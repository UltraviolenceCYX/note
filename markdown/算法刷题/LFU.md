# LFU

## 题目要求：

1、调用`get(key)`方法时，要返回该`key`对应的`val`。

2、只要用`get`或者`put`方法访问一次某个`key`，该`key`的`freq`就要加一。

3、如果在容量满了的时候进行插入，则需要将`freq`最小的`key`删除，如果最小的`freq`对应多个`key`，则删除其中最旧的那一个。

## code:

​	思路：维持三个map，记录值，频率，和同频率对应的键

```java
class LFUCache {
    private HashMap<Integer,Integer> keyToVal;
    private HashMap<Integer,Integer> keyToFreq;
    private HashMap<Integer,LinkedHashSet<Integer>> freqToKeys;

    private int minFreq;

    private int cap;

    public LFUCache(int capacity) {
        keyToFreq = new HashMap<>();
        keyToVal = new HashMap<>();
        freqToKeys = new HashMap<>();

        this.cap = capacity;
        this.minFreq = 0;

    }
    
    public int get(int key) {
        if(keyToVal.containsKey(key)){
            increaseFreq(key);
            return keyToVal.get(key);
        }
        return -1;


    }
    
    public void put(int key, int value) {
        if(this.cap <= 0 ) return;

        if(keyToVal.containsKey(key)) {
            keyToVal.put(key,value);
            increaseFreq(key);
            return;
        }

        if(this.cap <= keyToVal.size()){
            removeMinFreqKey();
        }

        keyToVal.put(key,value);
        keyToFreq.put(key,1);
        freqToKeys.putIfAbsent(1,new LinkedHashSet<>());
        freqToKeys.get(1).add(key);
        this.minFreq =1;

    }


    private void removeMinFreqKey(){
        LinkedHashSet<Integer> keyList = freqToKeys.get(this.minFreq);
        int deletedKey = keyList.iterator().next();

        keyList.remove(deletedKey);
        if (keyList.isEmpty()){
            freqToKeys.remove(this.minFreq);
        }

        keyToVal.remove(deletedKey);
        keyToFreq.remove(deletedKey);

    }

    private void increaseFreq(int key){
        int freq = keyToFreq.get(key);

        keyToFreq.put(key,freq+1);
        freqToKeys.get(freq).remove(key);
        freqToKeys.putIfAbsent(freq + 1, new LinkedHashSet<>());
        freqToKeys.get(freq + 1).add(key);
        if (freqToKeys.get(freq).isEmpty()) {
        freqToKeys.remove(freq);
        // 如果这个 freq 恰好是 minFreq，更新 minFreq
        if (freq == this.minFreq) {
            this.minFreq++;
        }
    }
    }
}

```

