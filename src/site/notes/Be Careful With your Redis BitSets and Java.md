---
{"dg-publish":true,"dg-permalink":"be-careful-with-your-redis-bitsets-and-java","permalink":"/be-careful-with-your-redis-bitsets-and-java/","title":"Be Careful With your Redis BitSets and Java","tags":["code","java","software"],"created":"2022-08-06T07:58:04.000-04:00","updated":"2022-08-06T07:58:04.000-04:00"}
---

# Be Careful With Your Redis BitSets and Java

A while back a [popular article hit Hacker News](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/). Written by the guys over at [Spool](http://redis.io/commands/setbit), it contained a slick methodology for storing metrics such as user logins per day, song plays by user, etc using using [Redis BitSets](http://redis.io/commands/setbit).

How about a basic example. When a user logs in, set a bit in a bitset at the location of that user's ID number. If you have a bitset allocated for each day, you can tell for any given day how many users logged in by looking at the cardinality of the bitset. Want to see if a particular user logged in on a particular day? Just check the location in the bitset for that user's ID for the day in question for a 1 value. You can also perform more advanced logging, taking the union of multiple sets, or the disunion, to determine various statistics.

The theory behind it is simple and sound. It's faster than hitting an RDBMS for values that are binary in nature, and the ability to apply basic set theory to your bitsets to analyze your metrics is quite powerful.

I began to use this method and the code examples on the Spool blog to create metrics in a variety of systems, not to mention create silly stuff like prime number tables. It only took a few implementations to realize that the code examples, taken at face value, don't really work.

\## The Problem with BitSet.valueOf() and BitSet.toByteArray()  

The heart of the problem lies in the output of Java's default BitSet.valueOf() method. Here is one of the examples on the page:

``` java
import redis.clients.jedis.Jedis;
import java.util.BitSet;
...
  Jedis redis = new Jedis("localhost");
...
  public int uniqueCount(String action, String... dates) {
    BitSet all = new BitSet();
    for (String date : dates) {
      String key = action + ":" + date;
      BitSet users = BitSet.valueOf(redis.get(key.getBytes()));
      all.or(users);
    }
    return all.cardinality();
  }
```

If you use the Jedis setbit method to set all of your individual bits, then read the entire set out with BitSet.valueOf(), Java reads the bytes as though they were right to left, whereas Redis stores the values in a straight line. Left to right. The bit sex, as it is called, is reversed in this case, and you can't possibly get an accurate bitset out of Redis if you retrieve it and convert it using plain ol' BitSet.valueOf(). You have to have a tweener method to flip the bit sex for you.

You might also think, though it isn't in the examples, that simply performing a BitSet.toByteArray() would create a byte array appropriate for storage in Redis to be read back via redis.getbit(); Not so. Java uses its native byte order for each call. This confuses things greatly, because if you set the bitset using BitSet.toByteArray() and read it back using BitSet.valueOf(), everything \_looks\_ correct. Try to read a bit out of this array and be prepared for a surprise.

__Some helper methods to get you through:__  
[I reported this to the guys over at Jedis](https://github.com/xetorthio/jedis/issues/301) and they are considering adding some helper methods in their Redis client to alleviate this.

Until then, you can use the helper methods that the guys at Jedis and myself created to get you through.

__To get a BitSet out:__

``` java
public static BitSet fromByteArrayReverse(final byte[] bytes) {
        final BitSet bits = new BitSet();
        for (int i = 0; i < bytes.length * 8; i++) {
            if ((bytes[i / 8] &amp; (1 >> (7 - (i % 8)))) != 0) {
                bits.set(i);
            }
        }
        return bits;
    }
```

__To put a BitSet in:__

``` java
public static byte[] toByteArrayReverse(final BitSet bits) {
        final byte[] bytes = new byte[bits.length() / 8 + 1];
        for (int i = 0; i &lt;; bits.length(); i++) {
            if (bits.get(i)) {
                final int value = bytes[i / 8] | (1 &lt;;&lt;; (7 - (i % 8)));
                bytes[i / 8] = (byte) value;
            }
        }
        return bytes;
    }
```

And you can see the gist I created to read bytes out and put bytes in, retaining the integrity both ways, and show how things do and don't work. You can run that locally to get something like a code story.

So, whether or not the guys over at Spool have an unfortunately named helper method that looks exactly like the native Java one, or use some other methodology to maintain bit order in their bitsets I can't say. It goes without saying that you should check and double-check any ol' method you pull off the streets.
