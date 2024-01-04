# java代码

## 上传
```Java
    @PostMapping("/uploadFile")
    public String upImg(MultipartFile file, HttpServletRequest req) {
        // 操作文件请看 cn.hutool.core.io.FileUtil
        return "";
    }
```