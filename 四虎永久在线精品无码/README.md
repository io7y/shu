catch (Exception ex)
        {
            Console WriteLine($"刻录失败: {ex Message}");
        }
    }
}
2  使用libburn库进行Linux下光碟刻录（C示例）
c
#include <libburn/libburn h>
 
void burn_disc(const char *device_path, const char *image_path) {
    struct burn_drive *drive;
    struct burn_source *source;
    struct burn_disc *disc;
    int ret;
    
    // 初始化libburn
    burn_initialize();
    
    // 打开设备
    drive = burn_drive_scan_and_grab(device_path, BURN_DRIVE_ANY, 0);
    if (!drive) {
        fprintf(stderr, "无法打开设备\n");
        return;
    }
    
    // 创建源（从文件）
    source = burn_file_source_new(image_path, 0);
    if (!source) {
        fprintf(stderr, "无法创建源文件\n");
        burn_drive_release(drive, 0);
        return;
    }
