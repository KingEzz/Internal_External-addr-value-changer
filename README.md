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
