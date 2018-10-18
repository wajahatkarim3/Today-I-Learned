# Encrypt / Decrypt Strings in Android

### Third-party Lib
A better and more secure way is implemented in [this library](https://github.com/tozny/java-aes-crypto). 


### Encrypt Strings

Please copy the [AESUtils](#aesutils-class) class in your project first and then you can use it like this.

```java
String encrypted = "";
String sourceStr = "This is any source string";
try {
    encrypted = AESUtils.encrypt(sourceStr);
    Log.d("TEST", "encrypted:" + encrypted);
} catch (Exception e) {
    e.printStackTrace();
}
```

### Decrypt Strings
Please copy the [AESUtils](#aesutils-class) class in your project first and then you can use it like this.

```java
String encrypted = "ANY_ENCRYPTED_STRING_HERE";
String decrypted = "";
try {
    decrypted = AESUtils.decrypt(encrypted);
    Log.d("TEST", "decrypted:" + decrypted);
} catch (Exception e) {
    e.printStackTrace();
}
```


### AESUtils Class

The easiest way of implementing [AES Encryption and Decryption](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) in Android is to copy this class in your projects.

```java
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;

public class AESUtils 
{

    private static final byte[] keyValue =
            new byte[]{'c', 'o', 'd', 'i', 'n', 'g', 'a', 'f', 'f', 'a', 'i', 'r', 's', 'c', 'o', 'm'};


    public static String encrypt(String cleartext)
            throws Exception {
        byte[] rawKey = getRawKey();
        byte[] result = encrypt(rawKey, cleartext.getBytes());
        return toHex(result);
    }

    public static String decrypt(String encrypted)
            throws Exception {

        byte[] enc = toByte(encrypted);
        byte[] result = decrypt(enc);
        return new String(result);
    }

    private static byte[] getRawKey() throws Exception {
        SecretKey key = new SecretKeySpec(keyValue, "AES");
        byte[] raw = key.getEncoded();
        return raw;
    }

    private static byte[] encrypt(byte[] raw, byte[] clear) throws Exception {
        SecretKey skeySpec = new SecretKeySpec(raw, "AES");
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, skeySpec);
        byte[] encrypted = cipher.doFinal(clear);
        return encrypted;
    }

    private static byte[] decrypt(byte[] encrypted)
            throws Exception {
        SecretKey skeySpec = new SecretKeySpec(keyValue, "AES");
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.DECRYPT_MODE, skeySpec);
        byte[] decrypted = cipher.doFinal(encrypted);
        return decrypted;
    }

    public static byte[] toByte(String hexString) {
        int len = hexString.length() / 2;
        byte[] result = new byte[len];
        for (int i = 0; i < len; i++)
            result[i] = Integer.valueOf(hexString.substring(2 * i, 2 * i + 2),
                    16).byteValue();
        return result;
    }

    public static String toHex(byte[] buf) {
        if (buf == null)
            return "";
        StringBuffer result = new StringBuffer(2 * buf.length);
        for (int i = 0; i < buf.length; i++) {
            appendHex(result, buf[i]);
        }
        return result.toString();
    }

    private final static String HEX = "0123456789ABCDEF";

    private static void appendHex(StringBuffer sb, byte b) {
        sb.append(HEX.charAt((b >> 4) & 0x0f)).append(HEX.charAt(b & 0x0f));
    }
}
```
