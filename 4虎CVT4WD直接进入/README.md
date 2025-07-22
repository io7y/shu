CloseHandle(deviceHandle);
    return 0;
}
2  ISO9660文件系统解析（Python示例）
python
import struct
 
class ISO9660Parser:
    def __init__(self, file_path):
        self file = open(file_path, 'rb')
        self primary_volume_descriptor = None
        self find_primary_volume_descriptor()
    
    def find_primary_volume_descriptor(self):
        self file seek(0x10)  # 跳过16字节的卷描述符类型
        while True:
            descriptor_type = self file read(1)
            if not descriptor_type:
                break
            
            if descriptor_type == b'\x01':  # 主卷描述符
                self primary_volume_descriptor = self parse_primary_volume_descriptor()
                break
            else:
                # 跳过其他描述符
                self file seek(2047, 1)  # 每个描述符2048字节
    
    def parse_primary_volume_descriptor(self):
        self file seek(-2048, 1)  # 回到描述符开始处
        
        # 读取基本字段
        type_code = self file read(1)
        identifier = self file read(5)
        version = self file read(1)
