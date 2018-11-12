(I AM NOT A C++ GOD LIKE YOU ALL MAY THINK I AM, DONT ACUSE ME FOR MY CODE / HOW I CODE *cough cough everyone (Denality / Cyberhound*)

So, We Need To Modify an Addresses Value, Lets Start By Creating The Function Itself.

```c++
#define ShortProtect(addr, NEW_PROTECTION, OLD_PROTECTION)(VirtualProtect(addr, sizeof(int), NEW_PROTECTION, OLD_PROTECTION))

namespace Memeory // Namespace Creation
{
	void ChangeAddrValue(DWORD addr, DWORD newValue, bool change_back_perms, bool change_back_value) // Creating The Function
	{
		DWORD old; // Old Protection
		DWORD past_value = 0; // Past Value
		ReadProcessMemory(GetCurrentProcess(), (LPCVOID)addr, (LPVOID)past_value, sizeof(DWORD), 0); // Reads The Old Data
		ShortProtect((LPVOID)addr, PAGE_EXECUTE_READWRITE, &old); // Modifies The Protection So We Can Write To It
		*(BYTE*)addr = newValue; // Changes the Value Of The Addr
		if (change_back_value == true) // Extra Shit
			WriteProcessMemory(GetCurrentProcess(), (LPVOID)addr, (LPVOID)past_value, sizeof(DWORD), 0); // Changes Back Value
		if (change_back_perms == true) // Extra Shit
			ShortProtect((LPVOID)addr, old, &old); // Changes Back Protection
		return;
	}
};

```

Alright Now That We Created The Function For The Value Change, We Need To Use This Inside a Lua C Function. 
Lets See How We Do This;

```c++
inline void getfield(DWORD luaState, int INDEX, const char *k)
{
	try
	{
		DWORD old;
		Memeory::ChangeAddrValue(agetfield, 0xEB, false, false); // Changes The Jump
		rgetfield(luaState, INDEX, k); // Calls Getfield
		Memeory::ChangeAddrValue(agetfield, 0x72, false, false); // Changes Back The Value Or Memcheck Is Triggered
		ShortProtect((LPVOID)agetfield, old, &old); // Changing Protection Back To Normal 
	}
	catch (exception error) // Exception Handling 
	{
		printf("LyonX Exception: %i\n", error);
	}
}
```

Now That Thats Done, You May Now Use Your EXPLOIT Bypassing Retcheck!

Credits:
- KingEzz (Creator / Coder)
