# 一个SpringBoot结合Swagger写接口文档的例子

## Swagger接入

在原有的SpringBoot中接入Swagger

### 第一步。
在pom.xml中添加maven依赖

```
<!--swagger库-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

### 第二步。

创建Swagger2配置类 , 代码如下.
请手动修改包名等信息

```
@Configuration
@EnableSwagger2
public class Swagger2 {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                //这里要改为自己项目包名
                .apis(RequestHandlerSelectors.basePackage("com.example.demo"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("在SpringBoot项目结合Swagger编写接口文档")
                .description("Swagger官方仓库https://github.com/swagger-api/swagger-ui")
                .termsOfServiceUrl("https://github.com/forgot2015/SpingBootAndSwaggerDemo")
                .version("1.1.0")
                .build();
    }
}
```

### 第三步。

在Controller类中编写接口注解内容

添加类注解

````
@Api(value="/test", tags="Swagger测试接口模块")
````

添加请求方法注解

````
@ApiOperation(value="根据Id查询用户信息", notes = "根据Id查询用户信息,这里是详细说明")
@ApiImplicitParam(name="id", value="用户唯一id", required = true, dataType = "Long")
````

比如：

````
@Api(value="/test", tags="Swagger测试接口模块")
@RestController
@RequestMapping("/api")
public class UserController {
    private static final Logger LOGGER = LoggerFactory.getLogger(UserController.class);

    @Autowired
    UserService userService;

    @ApiOperation(value="根据Id查询用户信息", notes = "根据Id查询用户信息,这里是详细说明")
    @ApiImplicitParam(name="id", value="用户唯一id", required = true, dataType = "Long")
    @RequestMapping(value="/user",params = "id",method = RequestMethod.GET)
    public User getUserById(@RequestParam Long id){
        return userService.findById(id);
    }

    @ApiOperation(value="根据Id添加用户信息", notes = "根据Id添加用户信息,这里是详细说明")
    @ApiImplicitParam(name="id", value="用户唯一id", required = true, dataType = "Long")
    @RequestMapping(value="/user",params = "id",method = RequestMethod.POST)
    public User addUser(@RequestParam Long id){
        return userService.addUser(id);
    }
}
````

最后，访问http://localhost:8089/swagger-ui.html 即可看到你的杰作

