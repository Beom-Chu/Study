# ChunkProvider, ChunkProcessor

Chunk 지향 처리 방식에 사용되는 핵심 인터페이스.

* **ChunkProvider**
  * `ItemReader`에서 청크(chunk)를 제공하는 역할을 담당합니다. 
    * 청크 : 한 번에 처리되는 아이템의 그룹
  * `ChunkProvider`는 다음 청크를 읽어오기 위해 `ItemReader`에서 호출되며, 청크의 크기와 청크 내의 아이템을 목록으로 반환 합니다.
* **ChunkProcessor**
  * 청크를 처리하는 역할을 담당합니다.
  * `ChunkProvider`로부터 제공된 청크를 받아 `ItemProcessor`와 `ItemWriter`를 사용하여 처리합니다.

`ChunkProvider`와 `CunkProcessor`는 일반적으로 `ChunkOrientedTasklet`과 함께 사용됩니다.

`ChunkOrientedTasklet`은 Spring Batch에서 제공하는 일반적인 청크 지향 처리를 위한 `Tasklet` 구현체 입니다.

`ChunkOrientedTasklet`은 `ChunkProvider`와 `CunkProcessor`를 조합하여 청크 단위로 처리를 수행합니다.

이를 통해 대량의 데이터를 청크 단위로 나누어 처리하며, 성능을 향상시킬 수 있습니다.

### 예시 코드

```java
public class MyChunkProvider implements ChunkProvider<String> {
    private final ItemReader<String> reader;
    private final int chunkSize;
    
    public MyChunkProvider(ItemReader<String> reader, int chunkSize) {
        this.reader = reader;
        this.chunkSize = chunkSize;
    }
    
    @Override
    public Chunk<String> provide() throws Exception {
        List<String> items = new ArrayList<>();
        for (int i = 0; i < chunkSize; i++) {
            String item = reader.read();
            if (item != null) {
                items.add(item);
            } else {
                break;
            }
        }
        return new Chunk<>(chunkSize, items);
    }
}

public class MyChunkProcessor implements ChunkProcessor<String> {
    private final ItemProcessor<String, String> processor;
    private final ItemWriter<String> writer;
    
    public MyChunkProcessor(ItemProcessor<String, String> processor, ItemWriter<String> writer) {
        this.processor = processor;
        this.writer = writer;
    }
    
    @Override
    public void process(Chunk<String> chunk) throws Exception {
        List<String> processedItems = new ArrayList<>();
        for (String item : chunk.getItems()) {
            String processedItem = processor.process(item);
            processedItems.add(processedItem);
        }
        writer.write(processedItems);
    }
}

@Bean
public Step myStep(ItemReader<String> reader, ItemProcessor<String, String> processor, ItemWriter<String> writer) {
    MyChunkProvider chunkProvider = new MyChunkProvider(reader, 10);
    MyChunkProcessor chunkProcessor = new MyChunkProcessor(processor, writer);
    
    ChunkOrientedTasklet<String> tasklet = new ChunkOrientedTasklet<>(chunkProvider, chunkProcessor);
    
    return stepBuilderFactory.get("myStep")
        .tasklet(tasklet)
        .build();
}
```

위의 예시에서 `MyChunkProvider`는 `ItemReader`에서 아이템을 읽어와서 청크를 생성합니다. 

`MyChunkProcessor`는 `ItemProcessor`와 `ItemWriter`를 사용하여 청크 내의 아이템을 변환하고 쓰는 작업을 수행합니다. 

`ChunkOrientedTasklet`을 사용하여 청크 단위로 처리되는 `Step`을 구성합니다.