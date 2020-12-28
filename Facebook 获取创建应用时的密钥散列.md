Facebook 获取创建应用时的密钥散列
-------------

### 解决方案:

> 推荐方式：直接在 Android 手机中使用代码生成 `密钥散列`

```
	// 获取FB需要的密钥散列
	private void facebookSign() {
		try {
			PackageInfo info = getPackageManager()
					.getPackageInfo(getPackageName(), PackageManager.GET_SIGNATURES);
			for (Signature signature : info.signatures) {
				MessageDigest md = MessageDigest.getInstance("SHA");
				md.update(signature.toByteArray());
				byte[] key = Base64.encodeBase64(md.digest());
				Log.i(TAG, "facebook key = " + new String(key));
			}
		} catch (NameNotFoundException e) {
				Log.i(TAG, "facebook key NameNotFoundException");
		} catch (NoSuchAlgorithmException e) {
				Log.i(TAG, "facebook key NoSuchAlgorithmException");
		}
	}


```

### 拓展阅读

[推荐文章](https://blog.csdn.net/zyp_jiushu/article/details/86705387)