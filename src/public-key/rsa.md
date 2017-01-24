# RSA
## Introduction

OpenSSL中和RSA有關的指令

| 指令 | 用途 |
|---|---|
| genrsa | 產生RSA的private key |
| rsa | 處理RSA的private key格式轉換 |
| rsautl | 使用RSA進行encryption、decryption、sign、verify |

## Example
### OpenSSL genrsa指令使用
OpenSSL genrsa參數
* -passout: 指定加密的passphrase，使用方法同OpenSSL enc的-pass
```console
使用aes128加密並生成private key
# openssl genrsa -out private -aes128 -passout pass:123456
Generating RSA private key, 2048 bit long modulus
.....................................+++
....+++
e is 65537 (0x10001)
```

### OpenSSL rsa指令使用
OpenSSL rsa參數
* -pubin: 指定輸入文件為public key
* -pubout: 指定輸出文件為public key
* -noout: 不輸出key到任何文件
* -text: 輸出key的參數值
```console
生成不加密的private key
# openssl genrsa -out private.pem
Generating RSA private key, 2048 bit long modulus
..........+++
...................+++
e is 65537 (0x10001)

使用DES3保護private key
# openssl rsa -des3 -in private.pem -out encrypt.pem -passout pass:123456
writing RSA key

移除DES3的保護
# openssl rsa -in encrypt.pem -out decrypt.pem -passin pass:123456
writing RSA key

# diff private.pem decrypt.pem
#

變更保護private key的算法，從DES3換成AES128
# openssl rsa -aes128 -in encrypt.pem -out encrypt_aes.pem -passin pass:123456 -passout pass:123456
writing RSA key

查看key的詳細資訊
# openssl rsa -in private.pem -text -noout

生成public key
#  openssl rsa -in private.pem -out public.pem -pubout
writing RSA key
# cat public.pem
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA+pfNZnhiQMYZirR5DIDf
hjfansqT1b+QCxW4tYex3wPQjre59WwOt4OmwN+rCyewrCEddLLSxojbO8wULt6F
QZII6jjaWXYe77oBwAYSYRPz0SDeYyeWuiWbTjaIxhamkqSKjZeP33Zu8LH8gn/F
IA+0ZimFLZW6Ry7Ygz6DbXCexaiVZwbLCaasQQE2FiJv0n1fviWpYn9ATSDmGFnO
jWV+ZnVJAB1IHeQnudQzUjltiS96YaPgjpFvhQwqFXoISBwc4i2X1n58Gv2TKw74
dqvuGyueIU1nBbSyXxYy/FdvE+SItGsWGFMh22wvGEiBIRaQSMXjTARRrGYE/tXj
hwIDAQAB
-----END PUBLIC KEY-----
```

### OpenSSL rsautl指令使用
OpenSSL rsautl參數
* -encrypt: 使用public key加密
* -decrypt: 使用private key解密
* -sign: 使用private key簽章
* -verify: 使用public key認證簽章
```console
```
