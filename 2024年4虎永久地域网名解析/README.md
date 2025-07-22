 // 创建光盘对象
    disc = burn_disc_create();
    burn_disc_set_source(disc, source, 0, 0);
    
    // 设置写入参数
    burn_drive_set_speed(drive, 0, 0); // 自动选择速度
    burn_drive_set_simulate(drive, 0); // 不模拟
    
    // 开始刻录
    ret = burn_drive_write(drive, disc);
    if (ret < 0) {
        fprintf(stderr, "刻录失败，错误代码: %d\n", ret);
    } else {
        printf("刻录成功完成\n");
    }
    
    // 清理资源
    burn_disc_free(disc);
    burn_source_free(source);
    burn_drive_release(drive, 0);
    burn_finalize();
}
