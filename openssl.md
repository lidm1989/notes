# **Windows下openssl**

# 对应
Windows下的libeay32.lib、ssleay32.lib分别对应Linux下的libcrypto.so、libssl.so文件
libssl32.dll与ssleay32.dll、libeay32.dll与libcrypto.dll的实际内容没有任何区别

# 下载
http://www.slproweb.com/products/Win32OpenSSL.html

# openssl genrsa
    openssl genrsa 1024 > private.key

# openssl rsa
    openssl rsa -in private.key -pubout > public.key 

# openssl req
    openssl req -new -key private.key -out my.csr

# ssh-keygen and openssl

# 单向认证 and 双向认证
### 单向认证
    package main

    import (
        "crypto/tls"
        "fmt"
    )

    func main() {
        tlsConfig := &tls.Config{
            //InsecureSkipVerify: true,
        }
        conn, err := tls.Dial("tcp", "www.baidu.com:443", tlsConfig)
        if err != nil {
            panic("failed to connect: " + err.Error())
        }
        defer conn.Close()
        fmt.Println("OK")
    }

* 不验证服务器证书：`InsecureSkipVerify: true`
* 使用系统预装ca验证服务器证书：`InsecureSkipVerify: false`(default)
* 使用自定义CA验证服务器证书：
    rootPEM := ""
    roots := x509.NewCertPool()
	ok := roots.AppendCertsFromPEM([]byte(rootPEM))
    tlsConfig := &tls.Config{
        RootCAs: roots,
        //ServerName: "example.com",
    }

### 双向认证
    cert, _ := tls.LoadX509KeyPair("lidm.crt", "private.key")
    rootPEM, _ := ioutil.ReadFile("ca.crt")
    roots := x509.NewCertPool()
	ok := roots.AppendCertsFromPEM([]byte(rootPEM))
    tlsConfig := &tls.Config{
        RootCAs: roots,
        //ServerName: "example.com",
        Certificates: []tls.Certificate{cert},
        //InsecureSkipVerify: true,
    }



