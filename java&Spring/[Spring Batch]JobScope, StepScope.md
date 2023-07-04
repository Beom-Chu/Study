# [Spring Batch] JobScope, StepScope

### JobScope

JobScope 어노테이션은 Spring Batch의 Job 인스턴스의 수명 범위를 지정합니다. JobScope를 사용하면 Job의 실행 동안 상태를 유지할 수 있습니다. 예를 들어, Job의 시작 시간, 실행 횟수 또는 사용자가 지정한 매개 변수와 같은 정보를 유지할 수 있습니다.

JobScope를 사용하기 위해서는 Spring의 컨텍스트에서 JobLauncher와 함께 Job을 실행하는 클래스에 `@EnableBatchProcessing` 어노테이션이 지정되어 있어야 합니다. 그리고 Job을 구성하는 클래스나 메소드에 `@JobScope` 어노테이션을 추가하여 Job의 수명 범위를 설정할 수 있습니다.

예를 들어:

```java
@Configuration
@EnableBatchProcessing
public class MyBatchConfiguration {
    
    @Autowired
    private JobBuilderFactory jobBuilderFactory;
    
    @Autowired
    private StepBuilderFactory stepBuilderFactory;
    
    @Bean
    @JobScope
    public Step myStep() {
        // Step 구성
    }
    
    @Bean
    public Job myJob() {
        return jobBuilderFactory.get("myJob")
            .start(myStep())
            .build();
    }
}
```

위의 예제에서 `myStep()` 메소드에 `@JobScope` 어노테이션이 추가되어 있으므로 해당 Step은 Job의 수명 범위 내에서 생성됩니다. 따라서 Step 내에서 유지해야 하는 상태 정보를 유지할 수 있습니다.



### StepScope

StepScope 어노테이션은 Spring Batch의 Step 인스턴스의 수명 범위를 지정합니다. StepScope를 사용하면 Step의 실행 동안 상태를 유지하거나 해당 Step에 특정한 매개 변수를 전달할 수 있습니다. StepScope를 사용하면 여러 Step 인스턴스를 동적으로 생성하고 구성할 수 있습니다.

StepScope를 사용하기 위해서는 Spring의 컨텍스트에서 JobLauncher와 함께 Step을 실행하는 클래스에 `@EnableBatchProcessing` 어노테이션이 지정되어 있어야 합니다. 그리고 Step을 구성하는 클래스나 메소드에 `@StepScope` 어노테이션을 추가하여 Step의 수명 범위를 설정할 수 있습니다.

예를 들어:

```java
@Configuration
@EnableBatchProcessing
public class MyBatchConfiguration {
    
    @Autowired
    private JobBuilderFactory jobBuilderFactory;
    
    @Autowired
    private StepBuilderFactory stepBuilderFactory;
    
    @Bean
    @StepScope
    public ItemReader<String> myItemReader(@Value("#{jobParameters['inputFile']}") String inputFile) {
        // ItemReader 구성
    }
    
    @Bean
    public Step myStep(ItemReader<String> reader, ItemProcessor<String, String> processor, ItemWriter<String> writer) {
        return stepBuilderFactory.get("myStep")
            .<String, String>chunk(10)
            .reader(reader)
            .processor(processor)
            .writer(writer)
            .build();
    }
    
    @Bean
    public Job myJob(Step myStep) {
        return jobBuilderFactory.get("myJob")
            .start(myStep)
            .build();
    }
}
```

위의 예제에서 `myItemReader()` 메소드에 `@StepScope` 어노테이션이 추가되어 있으므로 해당 ItemReader는 Step의 수명 범위 내에서 생성됩니다. 또한 `@Value("#{jobParameters['inputFile']}")`를 사용하여 Job 매개 변수를 읽어와서 ItemReader에 전달할 수 있습니다.

이렇게 함으로써 Step 내에서 특정한 상태 정보를 유지하거나 Step에 동적으로 매개 변수를 전달할 수 있습니다.

`@Value("#{jobExecutionContext['name1']}"` 를 사용해서 jobExecutionContext의 변수를 사용 하는 것도 가능합니다.

