# ExcelDownloader

## server - vo
```java
@Data
@AllCell(
    headerStyle = @CustomCellStyle(excelCellStyle = CustomHeaderCellStyle.class), 
    bodyStyle = @CustomCellStyle(excelCellStyle = CustomBodyCellStyle.class)
)
public class DemoVO {
    @Cell(headerName = "이메일")
    private String email;
    @Cell(headerName = "이름", headerStyle = @CustomCellStyle(excelCellStyle = RedCellStyle.class))
    private String name;
    @Cell(headerName = "나이", bodyStyle = @CustomCellStyle(excelCellStyle = BlueCellStyle.class))
    private String age;
    
    ...
}
```

## server - custom cell style
```java
public class GreyCellStyle implements ExcelCellStyle {

    @Override
    public void styleApply(CellStyle cellStyle) {
        cellStyle.setFillForegroundColor(IndexedColors.GREY_25_PERCENT.getIndex());
        cellStyle.setFillPattern(CellStyle.SOLID_FOREGROUND);
        
        ...
    }
}
```

## server - controller

```java
import utils.ExcelWriter;

@Controller
public class DemoController {
    @RequestMapping("/excelDownload.do")
    public void excelDownload(HttpServletResponse response) {
        List<DemoVO> demoList = new ArrayList<>();

        String[] email = {"abc@abc.io", "def@def.io"};
        String[] name = {"홍길동", "가나다"};
        String[] age = {"20", "25"};

        for (int i = 0; i < 2; i++) {
            DemoVO demo = new DemoVO();
            demo.setEmail(email[i]);
            demo.setName(name[i]);
            demo.setAge(age[i]);

            demoList.add(demo);
        }

        ExcelWriter excelWriter = new ExcelWriter();
        excelWriter.excelDownload(demoList, DemoVO.class, response, "demoExcel", false);
        ...
    }
}
```
