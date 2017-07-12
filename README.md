# Bean

```
@Configuration
public class KnightConfig {
    @Bean//注册为Spring应用上下文的bean实例，该注解不能省掉，此时创建bean的id与方法名一样，可以显示设置bean的id
    public Knight knight() {
        return new BraveKnight(quest());//进行依赖注入。
    }

    @Bean
    public Knight knight(Quest quest) {//使用这种方式进行注入，可以不依赖于quest方法创建。
        return new BraveKnight(quest);
    }
    @Bean
    public Quest quest() {
        return new SlayDragonQuest(System.out);
    }
}

<bean id="knight" class="actionsImpl.BraveKnight">
        <constructor-arg ref="dragon">
        </constructor-arg>
    </bean>
<bean id="dragon" class="actionsImpl.SlayDragonQuest">
       <constructor-arg value="#{T(System).out}"></constructor-arg>
</bean>
```

# Aop



```
@Component
@Aspect
public class Audience {
    //定义切点，可重复使用
    @Pointcut("execution(* com.muyundefeng.condition.AutoWire.bean.Performance.perform(..))")
    public void performace(){}
    @Before("performace()")
    public void silenceCellPhone(){
        System.out.println("turn off your cell phone");
    }

    @AfterReturning("performace()")
    public void appluad(){
        System.out.println("all audiences appluaed");
    }

    @AfterThrowing("performace()")
    public void refund(){
        System.out.println("please refund money");
    }
    @Around("performace()")
    public void aound(ProceedingJoinPoint point){
        System.out.println("silence your cellphone");
        try {
            point.proceed();
            System.out.println("applaud");
        } catch (Throwable throwable) {
            System.out.println("refund m,onet");
        }
    }
}

//配置文件
@Component
public class TalkShow implements Performance {
    @Override
    public void perform() {
        System.out.println("talk show is playing");
    }
}

@Configuration
@ComponentScan(basePackages = "com.muyundefeng.condition.AutoWire.bean")
@EnableAspectJAutoProxy
public class AspectConfig {
}

```

# spel

```
//@ Value注解可以放在字段，方法和方法/??构造 参数里，以指定默认值。    
@Autowired
    public Student(@Value("#{T(Math).PI*30}") double avgSore, @Value("#{music.name.toUpperCase()}") String muiscName,//引用别处bean的时候需要装配该bean
                   @Value("#{music.name eq 'jay'}") boolean isAdult, @Value("#{18 gt 12?true:false}") boolean isCollege,
                   @Value("#{music.name matches '.*jay.*'}") boolean isJay) {
        this.avgSore = avgSore;
        this.musicName = muiscName;
        this.isAdult = isAdult;
        this.isCollage = isCollege;
        this.isJay = isJay;
    }
```



