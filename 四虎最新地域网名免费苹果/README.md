// 读取数据
    if (!ReadFile(deviceHandle, buffer, bufferSize, &bytesReturned, NULL)) {
        std::cerr << "读取失败: " << GetLastError() << std::endl;
        return false;
    }
    
    if (bytesReturned != bufferSize) {
        std::cerr << "读取不完整" << std::endl;
        return false;
    }
    
    return true;
}
 
int main() {
    HANDLE deviceHandle = CreateFile(
        "\\\\ \\D:",         // 设备路径
        GENERIC_READ,         // 访问模式
        FILE_SHARE_READ | FILE_SHARE_WRITE, // 共享模式
        NULL,                // 安全属性
        OPEN_EXISTING,       // 创建选项
        FILE_FLAG_NO_BUFFERING | FILE_FLAG_RANDOM_ACCESS, // 标志
        NULL);               // 模板文件
    
    if (deviceHandle == INVALID_HANDLE_VALUE) {
        std::cerr << "无法打开设备: " << GetLastError() << std::endl;
        return 1;
    }
    
    BYTE buffer[2048]; // 标准扇区大小
    if (ReadSectorFromCD(deviceHandle, 16, buffer, sizeof(buffer))) {
        // 处理读取的数据（例如解析ISO9660文件系统）
        std::cout << "成功读取扇区" << std::endl;
    }
