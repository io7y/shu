
using IMAPI2;
using IMAPI2FS;
 
public class DiscBurner
{
    public void BurnDataDisc(string[] files, string driveLetter, string volumeName)
    {
        try
        {
            // 创建文件系统映像
            IFileSystemImage fileSystemImage = new FileSystemImage();
            fileSystemImage VolumeName = volumeName;
            
            // 添加文件到映像
            foreach (string file in files)
            {
                string destinationPath = System IO Path GetFileName(file);
                fileSystemImage Root AddTree(file, destinationPath);
            }
            
            // 创建映像
            IDiscMaster2 discMaster = new MsftDiscMaster2();
            for (int i = 0; i < discMaster Count; i++)
            {
                if (discMaster[i] == driveLetter + ":")
                {
                    IDiscRecorder2 discRecorder = new MsftDiscRecorder2();
                    discRecorder InitializeDiscRecorder(discMaster[i]);
                    
                    // 创建写入对象
                    IDiscFormat2Data discFormat = new MsftDiscFormat2Data();
                    discFormat Recorder = discRecorder;
                    discFormat ClientName = "MyDiscBurner";
                    discFormat ForceMediaToBeClosed = true;
                    
                    // 获取映像流
                    IMAPI2 IStream imageStream = fileSystemImage CreateResultImage() ImageStream;
                    
                    // 开始刻录
                    discFormat Write(imageStream);
                    break;
                }
            }
        }
