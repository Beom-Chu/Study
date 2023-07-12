# JPA 무한참조 방지

* Entity로 받고 Json직렬화 하기 전에 DTO 생성후 복사하기

  * [BeanUtils.copyProper 활용](./객체복사_BeanUtils.copyProperties.md) 
* 처음부터 DTO로 DB에서 받기
  * JPA Data에서 Projection 활용 
* @JsonIgnoreProperties({"board"})
  * 무시할 속성이나 속성 목록을 표시하는 데 사용
* @JsonIgnore
  * 필드 레벨에서 무시 될 수있는 속성을 표시
* @JsonBackReference @JsonManagedReference 

  * 부모 Class에 @JsonManagedReference를, 자식 Class에 @JsonBackReference를 Annotation에 추가
