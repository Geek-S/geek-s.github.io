---
title: "MapStruct"
date: 2022-01-01T20:55:28+08:00
draft: false
tags: ["java"]
categories: ["Java Foundation"]

showShare: false
---

# Quick start

```
public class Car {
 
    private String make;
    private int numberOfSeats;
    private CarType type;
}
```

```
public class CarDto {
 
    private String make;
    private int seatCount;
    private String type;
}
```

声明Mapper

```
@Mapper
public interface CarMapper {
 
    CarMapper INSTANCE = Mappers.getMapper( CarMapper.class );
 
    @Mapping(source = "numberOfSeats", target = "seatCount")
    CarDto carToCarDto(Car car);
}
```

转换

```
@Test
public void shouldMapCarToDto() {
    // given
    Car car = new Car( "Morris", 5, CarType.SEDAN );
 
    // when
    CarDto carDto = CarMapper.INSTANCE.carToCarDto( car );
 
    // then
    assertThat( carDto ).isNotNull();
    assertThat( carDto.getMake() ).isEqualTo( "Morris" );
    assertThat( carDto.getSeatCount() ).isEqualTo( 5 );
    assertThat( carDto.getType() ).isEqualTo( "SEDAN" );
}
```

# 参考

- [mapstruct.org](https://mapstruct.org)
- [mapstruct@github](https://github.com/mapstruct/mapstruct)
