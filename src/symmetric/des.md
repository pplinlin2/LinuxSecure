# DES
## Examples
### 輸入密碼的5種方式
```console
使用命令行輸入
# echo "hello world" > plain.txt
# openssl enc -des -in plain.txt -out encrypt.txt -pass pass:123456

使用檔案
# echo "123456" > passwd.txt
# openssl enc -des -in plain.txt -out encrypt.txt -pass file:passwd.txt

使用環境變量
# passwd=123456 openssl enc -des -in plain.txt -out encrypt.txt -pass env:passwd

使用file descriptor
# openssl enc -des -in plain.txt -out encrypt.txt -pass fd:0 < passwd.txt

使用stdin
# openssl enc -des -in plain.txt -out encrypt.txt -pass stdin < passwd.txt
```

### Passphrase及Salt效果
OpenSSL參數
* -P: 輸出key和IV值，不進行加解密
* -p: 輸出key和IV值
* -S: 指定salt值
```console
# openssl enc -des -pass pass:123456 -P
salt=76C45C15C0291149
key=41825B7F4CFD5E82
iv =FE1A492B9B4931F0
# openssl enc -des -pass pass:123456 -P
salt=2377E27F16BA0DC3
key=54534F53525C297D
iv =42E6672FE291F418

固定salt值，由passphrase產生的key和IV值就不會變化
# openssl enc -des -pass pass:123456 -P -S 123
salt=1230000000000000
key=50E1723DC328D98F
iv =133E321FC2908B78
#  openssl enc -des -pass pass:123456 -P -S 123
salt=1230000000000000
key=50E1723DC328D98F
iv =133E321FC2908B78

觀察DES的passphrase所產生的key和IV值，是經由md5計算而得的
# openssl enc -des -pass pass:123456 -P -nosalt
key=E10ADC3949BA59AB
iv =BE56E057F20F883E
# openssl dgst -md5 <(echo -n "123456")
MD5(/dev/fd/63)= e10adc3949ba59abbe56e057f20f883e
```

### 驗證DES加密算法
OpenSSL參數
* -K, -iv: 指定key和IV值

驗證由「0x0123456789ABCDEF」使用key「0x133457799BBCDFF1」經由DES加密後成為「0x85E813540F0AB405」
```console
# openssl enc -des -in <(echo -en "\x01\x23\x45\x67\x89\xab\xcd\xef") -out encrypt.txt -K 133457799BBCDFF1 -iv '' -p -nosalt -nopad
key=133457799BBCDFF1
iv =0000000000000000
# hexdump -C encrypt.txt
00000000  85 e8 13 54 0f 0a b4 05                           |...T....|
00000008
```
