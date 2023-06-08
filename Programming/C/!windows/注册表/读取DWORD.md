```c
  HKEY hKey;
	DWORD dwDisposition;
	int retval;
	DWORD dwOptions = REG_OPTION_NON_VOLATILE;
	LPCTSTR subKey = REG_BASE;
	long lRet = RegOpenKeyEx(HKEY_LOCAL_MACHINE, subKey, 0, KEY_WOW64_64KEY | KEY_READ, &hKey);
	if (lRet != ERROR_SUCCESS)
	{
		error(_F, _L, "open reg failed");
		RegCloseKey(hKey);
		return REG_STATUS_FAILED;
	}
	DWORD lpType = REG_SZ;

	unsigned char *data=(unsigned char *)calloc(4,sizeof (unsigned char *));
	DWORD length = 4;
	// https://learn.microsoft.com/zh-cn/windows/win32/api/winreg/nf-winreg-regqueryvalueexw
	lRet = RegQueryValueEx(hKey, REG_KEY_PROVIDER, NULL, &lpType,data, &length);


	if (lRet == ERROR_SUCCESS)
	{
		printf("%d",(*(int *) data));
	}
```

