# Android https post json using HttpsUrlConnection

.p12 生成 .crt文件命令
```
openssl pkcs12 -in filename.pfx -clcerts -nokeys -out filename.crt
```

代码参考

```java
		// Load CAs from an InputStream
		// (could be from a resource or ByteArrayInputStream or ...)
		CertificateFactory cf = CertificateFactory.getInstance("X.509", "BC");
		// From https://www.washington.edu/itconnect/security/ca/load-der.crt
		InputStream caInput = new BufferedInputStream(is);
		Certificate ca;
		try {
		    ca = cf.generateCertificate(caInput);
		} finally {
		    caInput.close();
		}

		// Create a KeyStore containing our trusted CAs
		String keyStoreType = KeyStore.getDefaultType();
		KeyStore keyStore = KeyStore.getInstance(keyStoreType);
		keyStore.load(null, null);
		keyStore.setCertificateEntry("ca", ca);

		// Create a TrustManager that trusts the CAs in our KeyStore
		String tmfAlgorithm = TrustManagerFactory.getDefaultAlgorithm();
		TrustManagerFactory tmf = TrustManagerFactory.getInstance(tmfAlgorithm);
		tmf.init(keyStore);

		// Create an SSLContext that uses our TrustManager
		SSLContext sslContext = SSLContext.getInstance("TLS");
		sslContext.init(null, tmf.getTrustManagers(), null);

		// Tell the URLConnection to use a SocketFactory from our SSLContext
		URL url = new URL("https://192.168.0.96:9443//sdk/update_stat?uniId="+ md5(id) + "&vendorId=" + "abcd1234");
		HttpsURLConnection urlConnection =
		    (HttpsURLConnection)url.openConnection();
		
		
		
		urlConnection.setSSLSocketFactory(sslContext.getSocketFactory());
		
		urlConnection.setHostnameVerifier(new HostnameVerifier() {
			
			@Override
			public boolean verify(String hostname, SSLSession session) {
				return true;
			}
		});
		
		urlConnection.setRequestMethod("POST");
		urlConnection.setDoOutput(true);
		urlConnection.setDoInput(true);
		urlConnection.setRequestProperty("Content-Type", "application/json");
		urlConnection.setRequestProperty("Accept", "application/json");
```
