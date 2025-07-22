
#include <windows h>
#include <ntddcdrm h>
#include <iostream>
 
bool ReadSectorFromCD(HANDLE deviceHandle, DWORD sectorNumber, BYTE* buffer, DWORD bufferSize) {
    CDROM_READ_TOC_EX readTocEx;
    DWORD bytesReturned;
    
    // 设置读取参数
    STORAGE_PROPERTY_QUERY query = {};
    query PropertyId = StorageDeviceProperty;
    query QueryType = PropertyStandardQuery;
    
    // 定位到指定扇区
    DWORD sectorSize = 2048; // 标准数据扇区大小
    LARGE_INTEGER sectorOffset;
    sectorOffset QuadPart = sectorNumber * sectorSize;
    
    if (SetFilePointerEx(deviceHandle, sectorOffset, NULL, FILE_BEGIN) == 0) {
        std::cerr << "定位扇区失败: " << GetLastError() << std::endl;
        return false;
    }
