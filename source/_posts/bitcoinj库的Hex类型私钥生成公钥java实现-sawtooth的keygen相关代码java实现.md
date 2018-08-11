---
title: bitcoinj库的Hex类型私钥生成公钥java实现-sawtooth的keygen相关代码java实现
date: 2018-08-07 23:44:30
tags:
    - 区块链
    - sawtooth
    - hyperledger
    - 密码学
    - java
---

## 分析  
- 我们在使用*hyperledger sawtooth*平台的时候，需要使用*keygen*这一个**sawtooth**的cli程序去创建一对密钥对，在*hyperledger sawtooth*中,该过程是使用一个特定的随机函数，生成一个私钥，再执行*Secp256k1*椭圆曲线算法，根据私钥导出公钥  
- 我在使用sawtooth的java sdk时候遇到了这样的问题，首先java的sdk 源码(https://github.com/hyperledger/sawtooth-sdk-java)  
  -  在sdk的example代码，调用了如下两个函数:  
    {% codeblock lang:java%}
    public static ECKey readWif(String wif) {
        return DumpedPrivateKey.fromBase58(MAINNET, wif).getKey();
    }

    public static String getPublicKey(ECKey privateKey) {
        return ECKey.fromPrivate(privateKey.getPrivKey(), true).getPublicKeyAsHex();
    }
    {% endcodeblock %}

  - 并且按照example给出的测试集其中一对公钥私钥如下:  
    {% codeblock %}
    私钥: 5JkisHAXScTk6Tah9jq9S5B4ByiputRjnKnQrF6k1uBLBQAD8Mi 
    公钥: 03bc976dc770b84decb90abeb94364c6368da051a9417cc4046f9f478d995b2ba7
    {% endcodeblock %}
  按照以上的调用，通过```readWif(String wif)``` 把私钥导入，确实能够生成以上对应的公钥，也证明了bitcoinj库是没有任何问题  

  - 同时我是用sawtooth自带的keygen工具去生成密钥对时候发现,sawtooth自带的生成公钥和私钥长度和以上测试用例的不一样，具体如下:  
    {% codeblock %}
    私钥 ：'20f34219eed055a8292767876e97cedc681cdee65f8d100f0c192d0b61cb13d6' 
    公钥 ：'02fe868857f1dcb31137b34c55cf1b6e031447b5fe7902e49a083eaf95c54aaecf'
    {% endcodeblock %}

- 通过分析我们发现，example给出的例子使用的经过了base58编码的公私密钥对，而sawtooth原生的keygen则没有采用base58编码。因此如果使用java的bitcoinj库现有的方法能生成sawtooth keygen格式的公钥私钥对呢？这个就是本文的一个重点。  


## 解决方法:  
1. 我们首先观察example里面的Signing 类，发现里面调用的方法都是来自于ECKey这个类的，通过分析，首先在ECKey类里面的私钥都是使用BigInteger表示的，而我们sawtooth keygen生成的私钥是16位Hex格式的String  
2. 因此我们需要使用方法把导入的String从16位Hex读入再变成BigInteger对象，再使用ECKey的fromPrivate((BigInteger privKey)方法，由BigInteger对象生成一个ECKey的对象。  
具体的fromPrivate()方法如下:  
{% codeblock lang:java%}
    public static ECKey fromPrivate(BigInteger privKey) {
        return fromPrivate(privKey, true);
    }
{% endcodeblock %}  
3. 最后通过getPublicKeyAsHex()方法即可得到由16位hex编码的String 公钥  

## 具体实现代码:  
{% codeblock getPublicKeyFromHex() lang:java%}
public static String getPublicKeyFromHex(String key){
    BigInteger privateKeyBigInt = new BigInteger(key,16);
    ECKey privateKey = ECKey.fromPrivate(privateKeyBigInt);
    return privateKey.getPublicKeyAsHex();
}
{% endcodeblock %} 

## 实现的例子:  
{% codeblock main.java lang:java%}
import org.bitcoinj.core.ECKey;
import java.math.BigInteger;

public class main {
    public static String getPublicKeyFromHex(String key){
        BigInteger privateKeyBigInt = new BigInteger(key,16);
        ECKey privateKey = ECKey.fromPrivate(privateKeyBigInt);
        return privateKey.getPublicKeyAsHex();
    }

    public static void main(String[] args) {

        //此处我输入我上文提到的公钥
        String privateKeyStr = "20f34219eed055a8292767876e97cedc681cdee65f8d100f0c192d0b61cb13d6";
        System.out.println(getPublicKeyFromHex(privateKeyStr));
    }
}
{% endcodeblock %} 
- 程序执行结果:  
{% codeblock 程序执行结果 %}
02fe868857f1dcb31137b34c55cf1b6e031447b5fe7902e49a083eaf95c54aaecf
{% endcodeblock %}
- 我们成功的使用java实现了sawtooth自带的keygen功能，除了keygen的通过随机数生成privatekey外，我们已经实现了把keygen生成的私钥生成回sawtooth keygen兼容的公钥格式



