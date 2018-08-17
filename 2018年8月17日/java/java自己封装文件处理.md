##### 主要封装了用字节流和字符流读取文件、写入文件、拷贝文件和拷贝文件夹四个功能。
```
public class myFileIO {
    private final int DATA_LENGTH = 1024;

    /**
     * 读取指定位置的文件内容，返回字符串
     *
     * @param srcFilePath 文件的路径
     * @return 文件的内容
     */
    public String readFile(String srcFilePath) {
        StringBuilder sb = new StringBuilder();
        //1.建立file对象
        File src = new File(srcFilePath);
        return readFile(src);
    }

    /**
     * 读取指定文件对象的内容到String中
     *
     * @param src 要读取的文件对象
     * @return 文件的内容
     */
    public String readFile(File src) {
        StringBuilder sb = new StringBuilder();
        //1.建立file对象
        if (!src.isFile()) {
            System.out.println("不是文件，无法读取");
            return null;
        }
        //2.选择流
        InputStream is = null;
        try {
            is = new BufferedInputStream(new FileInputStream(src));
            //3.读取
            byte[] bytes = new byte[DATA_LENGTH];
            int len = 0;
            while (-1 != (len = is.read(bytes))) {
                //输出
                String info = new String(bytes, 0, len);
                sb.append(info);
                //System.out.println(info);
            }
        } catch (java.io.FileNotFoundException e) {
            System.out.println("文件没有找到");
            e.printStackTrace();
        } catch (java.io.IOException e) {
            System.out.println("文件读取异常");
            e.printStackTrace();
        } finally {
            if (null != is) {
                try {
                    is.close();
                } catch (IOException e) {
                    System.out.println("文件关闭异常");
                    e.printStackTrace();
                } finally {
                    return sb.toString();
                }
            }
        }
        return null;
    }

    /**
     * 在指定位置的文件处写入指定的内容
     *
     * @param destFilePath 目标文件地址
     * @param content      要写入的内容，不可以超过DATA_LENGTH
     * @return true=写入成功，false=写入失败
     */
    public boolean writeFile(String destFilePath, String content) {
        //1.建立file对象
        File dest = new File(destFilePath);
        return writeFile(dest, content);
    }

    /**
     * 在指定的文件处写入指定的内容
     *
     * @param dest    目标文件
     * @param content 写入的内容，不可以超过DATA_LENGTH
     * @return true=写入成功，false=写入失败
     */
    public boolean writeFile(File dest, String content) {
        //1.建立file对象
        if (dest.isDirectory()) {
            //如果目标文件已经存在且是一个文件夹，是不可以创建的
            System.out.println("拷贝失败！");
            System.out.println("目标无法覆盖");
            return false;
        }
        //2.选择文件输出流
        OutputStream os = null;
        try {
            os = new BufferedOutputStream(new FileOutputStream(dest, true));
            //3. 操作
            os.write(content.getBytes(), 0, content.getBytes().length);
            //强制把缓冲区刷出
            os.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
            System.out.println("文件未找到");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("文件写出失败");
        } finally {
            //释放资源
            try {
                os.close();
                return true;
            } catch (IOException e) {
                e.printStackTrace();
                System.out.println("文件关闭失败");
            }
        }
        return false;
    }

    /**
     * 拷贝文件
     *
     * @param srcFilePath  源文件地址
     * @param destFilePath 目标文件地址
     * @return true=拷贝成功，false=拷贝失败
     */
    public boolean copyFile(String srcFilePath, String destFilePath) {
        //1. 建立源和目的地，确保源存在且为文件
        File src = new File(srcFilePath);
        File dest = new File(destFilePath);
        return copyFile(src, dest);
    }

    /**
     * 拷贝文件夹
     *
     * @param src  源文件
     * @param dest 目标文件
     * @return true=拷贝成功，false=拷贝失败
     */
    public boolean copyFile(File src, File dest) {
        //1. 建立源和目的地，确保源存在且为文件
        if (!src.isFile()) {
            System.out.println("拷贝失败！");
            System.out.println("源只能是文件");
            return false;
        }
        if (dest.isDirectory()) {
            //如果目标文件已经存在且是一个文件夹，是不可以创建的
            System.out.println("拷贝失败！");
            System.out.println("目标无法覆盖");
            return false;
        }
        //2.选择流
        InputStream is = null;
        OutputStream os = null;
        try {
            is = new BufferedInputStream(new FileInputStream(src));
            os = new BufferedOutputStream(new FileOutputStream(dest));
            //3.不断读取+写出 完成拷贝
            byte[] data = new byte[DATA_LENGTH];
            int len = 0;
            while (-1 != (len = is.read(data))) {
                os.write(data, 0, len);
            }
            os.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.先打开后关闭
            try {
                os.close();
                is.close();
                return true;
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return false;
    }

    /**
     * 拷贝文件夹
     *
     * @param srcFolderPath  源文件夹地址
     * @param destFolderPath 目的文件夹地址
     * @throws IOException
     */
    public void copyFolder(String srcFolderPath, String destFolderPath) {
        //第一步，先把最外面的那个文件夹拷贝过去(其实是创建一个新的)
        File src = new File(srcFolderPath);
        File dest = null;
        if (src.isDirectory()) {
            dest = new File(destFolderPath, src.getName());
        }
        //第二步，把该文件夹内的内容拷贝过去
        copyDir(src, dest);
    }

    /**
     * 拷贝文件夹
     *
     * @param src  源文件夹对象
     * @param dest 目的文件夹对象
     */
    public void copyFolder(File src, File dest) {
        if (src.isDirectory()) {
            dest = new File(dest, src.getName());
        }
        copyDir(src, dest);
    }

    /**
     * 递归拷贝文件夹中的内容
     *
     * @param src  源文件对象
     * @param dest 目标文件夹对象
     */
    public void copyDir(File src, File dest) {
        if (src.isFile()) {
            copyFile(src, dest);
        } else if (src.isDirectory()) {
            //确保目标文件夹存在
            dest.mkdirs();
            for (File sub : src.listFiles()) {
                copyDir(sub, new File(dest, sub.getName()));
            }
        }
    }

    /**
     * 字符的读取，只能用于纯文本，注意字符集
     *
     * @param srcFilePath 源文件地址
     * @return 文件的内容
     */
    public String readFileByReader(String srcFilePath) {
        StringBuilder sb = new StringBuilder();
        //创建源
        File src = new File(srcFilePath);
        Reader reader = null;
        try {
            reader = new FileReader(src);
            char[] data = new char[DATA_LENGTH];
            int len = 0;
            while (-1 != (len = reader.read(data))) {
                //char转String
                String info = new String(data, 0, len);
                sb.append(info);
            }
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到");
            e.printStackTrace();
        } catch (IOException e) {
            System.out.println("文件读取异常");
            e.printStackTrace();
        } finally {
            try {
                reader.close();
                return sb.toString();
            } catch (IOException e) {
                System.out.println("文件关闭异常");
                e.printStackTrace();
            }
        }
        return null;
    }

    /**
     * 利用buffer来提高效率，仍然只能用于纯文本
     *
     * @param srcFilePath 源文件地址
     * @return 文件的内容
     */
    public String readFileByBufferedReader(String srcFilePath) {
        StringBuilder sb = new StringBuilder();
        //创建源
        File src = new File(srcFilePath);
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(src));
            String line = null;
            while (null != (line = reader.readLine())) {
                sb.append(line);
            }
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到");
            e.printStackTrace();
        } catch (IOException e) {
            System.out.println("文件写出异常");
            e.printStackTrace();
        } finally {
            try {
                reader.close();
                return sb.toString();
            } catch (IOException e) {
                System.out.println("文件关闭异常");
                e.printStackTrace();
            }
        }
        return null;
    }

    /**
     * 向指定的文件中写入内容
     *
     * @param destFilePath 目标文件地址
     * @param content      要写入的内容
     */
    public void writeFileByWriter(String destFilePath, String content) {
        File dest = new File(destFilePath);
        Writer writer = null;
        try {
            writer = new FileWriter(dest);
            writer.write(content);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                writer.close();
            } catch (IOException e) {
                System.out.println("文件关闭异常");
                e.printStackTrace();
            }
        }


    }

    /**
     * 向指定的文件中写入内容
     *
     * @param destFilePath 目标文件地址
     * @param content      要写入的内容
     */
    public void writeFileByBufferedWriter(String destFilePath, String content) {
        File dest = new File(destFilePath);
        BufferedWriter writer = null;
        try {
            writer = new BufferedWriter(new FileWriter(dest));
            writer.write(content);
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                writer.close();
            } catch (IOException e) {
                System.out.println("文件关闭异常");
                e.printStackTrace();
            }
        }


    }

    /**
     * 拷贝纯文本的文件
     *
     * @param srcFilePath  源文件的地址
     * @param destFilePath 目标文件的地址
     * @return true = 成功，false = 失败
     */
    public boolean copyPlainTextFile(String srcFilePath, String destFilePath) {
        File src = new File(srcFilePath);
        File dest = new File(destFilePath);
        if (!src.isFile()) {
            System.out.println("拷贝失败！");
            System.out.println("源只能是文件");
            return false;
        }
        if (dest.isDirectory()) {
            //如果目标文件已经存在且是一个文件夹，是不可以创建的
            System.out.println("拷贝失败！");
            System.out.println("目标无法覆盖");
            return false;
        }
        BufferedReader reader = null;
        BufferedWriter writer = null;
        try {
            reader = new BufferedReader(new FileReader(src));
            writer = new BufferedWriter(new FileWriter(dest));
            String line = null;
            while (null != (line = reader.readLine())) {
                writer.write(line);
                writer.newLine();   //等价于writer.write("\r\n");
            }
            writer.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                writer.close();
                reader.close();
                return true;
            } catch (IOException e) {
                System.out.println("文件关闭异常");
                e.printStackTrace();
            }
        }
        return false;
    }
}
```

日后有空再来补全每个函数的详解。
