---
layout: mypost
title: 实体映射工具mapstruct
categories: [java]
---

### pom 引入

```
<dependency>
<groupId>org.mapstruct</groupId>
<!-- jdk8以下就使用mapstruct -->
<artifactId>mapstruct-jdk8</artifactId>
<version>1.2.0.Final</version>
</dependency>
<dependency>
<groupId>org.mapstruct</groupId>
<artifactId>mapstruct-processor</artifactId>
<version>1.2.0.Final</version>
</dependency>
```

### 例子

```
public class TestPo {
    private Integer id;
    private String brand;
}

public class TestVo {
    private Integer id;
    private String brand;
}

public class TestDto {
    private Integer id;
    private String brandName;
}

public class TestGroup {
    private Integer id;
    private String brand;
    private Integer id2;
    private String brandName;
}
```

```
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;
import org.mapstruct.factory.Mappers;
import java.util.List;

//@Mapper(componentModel="spring") 采取spring的方式
@Mapper
public interface TestCovertBasic {
	// 默认方式
    TestCovertBasic INSTANCE = Mappers.getMapper(TestCovertBasic.class);

    /**
     * TestPo TO TestVo
     * @param testPo
     * @return
     */
    TestVo toConvertVo(TestPo testPo);

    /**
     * TestPo TO TestDto
     * @param po
     * @return
     */
    @Mappings({
        @Mapping(source = "brand", target = "brandName")
    })
    TestDto toConvertDto(TestPo po);

    /**
     * List<TestPo> to List<TestVo>
     * @param pos
     * @return
     */
    List<TestVo> toConvertVos(List<TestPo> pos);

    /**
     * TestVo,TestDto to TestGroup
     * @param vo
     * @param dto
     * @return
     */
    @Mappings({
          @Mapping(source = "vo.id",target = "id"),
          @Mapping(source = "vo.brand",target = "brand"),
          @Mapping(source = "dto.id",target = "id2"),
          @Mapping(source = "dto.brandName",target = "brandName")
    })
    TestGroup toConvertGroup(TestVo vo,TestDto dto);
}
```

```
// 默认方式调用
TestGroup group = TestCovertBasic.INSTANCE.toConvertGroup(vo,dto);

// spring 的方式调用
@Autowired
private TestCovertBasic testCovertBasic;

TestGroup group = testCovertBasic.toConvertGroup(vo,dto);
```

