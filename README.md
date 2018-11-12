(I AM NOT A C++ GOD LIKE YOU ALL MAY THINK I AM, DONT ACUSE ME FOR MY CODE / HOW I CODE *cough cough everyone (Denality / Cyberhound*)

So, We Need To Modify an Addresses Value, Lets Start By Creating The Function Itself.

```c++
#define ShortProtect(addr, NEW_PROTECTION, OLD_PROTECTION)(VirtualProtect(addr, sizeof(int), NEW_PROTECTION, OLD_PROTECTION))

namespace Memeory
{
	void ChangeAddrValue(DWORD addr, DWORD newValue, bool change_back_perms, bool change_back_value, LPCWSTR Process)
	{
		DWORD old;
		DWORD past_value = 0;
		DWORD pid;
		if (Process == 0 || Process == NULL)
			GetWindowThreadProcessId(FindWindow(NULL, (LPCWSTR)"Roblox"), &pid);
		else
			GetWindowThreadProcessId(FindWindow(NULL, Process), &pid);
		ReadProcessMemory(OpenProcess(PROCESS_ALL_ACCESS,true, pid), (LPCVOID)addr, (LPVOID)past_value, sizeof(DWORD), 0);
		ShortProtect((LPVOID)addr, PAGE_EXECUTE_READWRITE, &old);
		*(BYTE*)addr = newValue;
		if (change_back_value == true)
			WriteProcessMemory(OpenProcess(PROCESS_ALL_ACCESS, true, pid), (LPVOID)addr, (LPVOID)past_value, sizeof(DWORD), 0);
		if (change_back_perms == true)
			ShortProtect((LPVOID)addr, old, &old);
		return;
	}
};


```

(Now, this can be modified to be used w/ External Processes Aswell!)

-=-=- Retcheck -=-=-
Alright Now That We Created The Function For The Value Change, Retcheck bypass would be similarly used by doing this. We First Need To Use This Inside a Lua C Function. 
Lets See How We Do This;

```c++
inline void getfield(DWORD luaState, int INDEX, const char *k)
{
	try
	{
		DWORD old;
		Memeory::ChangeAddrValue(agetfield, 0xEB, false, false, NULL);
		rgetfield(luaState, INDEX, k);
		Memeory::ChangeAddrValue(agetfield, 0x72, false, false, NULL);
		ShortProtect((LPVOID)agetfield, old, &old);
	}
	catch (exception error)
	{
		printf("LyonX Exception: 0x%i\n", error);
	}
}
```

Thats How To in theory bypass Retcheck.

This Can Also Be Used To Change Identity
```c++
inline void ChangeIdentity(DWORD value)
{
	Memeory::ChangeAddrValue(identity, 5, false, false, NULL);
	return;
}
```

-=- External Haxx -=-
Assault Cube:
`

Credits:
- KingEzz (Creator / Coder)
