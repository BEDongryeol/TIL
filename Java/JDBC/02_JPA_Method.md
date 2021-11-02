### findAll() 
- db의 전체 값을 가져오는 메서드
- 실제 서비스에서는 성능 적인 이슈로 잘 사용하지 않는다.

### findAll(Sort sort)
- findAll()를 정렬값을 통해 정렬할 수 있다.

### findAllById(Iterable<ID> ids)
- ID값을 Iteralbe한 List 값으로 받아 In 구문으로 조회한다.

### saveAll()
- Entity를 List 형식으로 받아서 한 번에 db에 저장한다.

### flush()
- jpa context에 있는 값을 db에 반영한다.

### saveAndFlush()
- 저장한 데이터를 바로 db에 반영한다.

### deleteInBatch()
- Entity를 Iterable 형식으로 받아서, 한번에 지운다.

### deleteAllInBatch()
- 조건에 상관없이 테이블을 통채로 지운다

### getOne(ID id)
- id에 해당하는 값을 호출한다.



