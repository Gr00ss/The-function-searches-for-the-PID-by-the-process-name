# The function searches for the PID by the process name
```C
uintptr_t getprocessid(const char* szprocessname){
    uintptr_t processid = 0;
    HANDLE hsnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS,0);
    if (hsnap != INVALID_HANDLE_VALUE)
    {
        PROCESSENTRY32 pe32;
        pe32.dwSize = sizeof(pe32);

        if(Process32First(hsnap, &pe32))
        {
            do
            {
                if(!_strcmpi(szprocessname, (const char*)pe32.szExeFile))
                {
                    processid = pe32.th32ProcessID;
                    break;
                }

            } while (Process32Next(hsnap,&pe32));
        }
    }
    if(hsnap)
        CloseHandle(hsnap);
    return processid;
}
```
