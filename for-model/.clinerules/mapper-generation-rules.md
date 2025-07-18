Mapper Interface

Class name should be specified in the format of {EntityName}Mapper
Use @Mapper annotation
Example:

@Mapper("orderMapper")
public interface OrderMapper {
    Order selectOrderById(OrderId orderId);
    void insertOrder(Order order);
    void updateOrder(Order order);
}

MyBatis Mapper XML

Located in src/main/resources/egovframework/mapper/{AggregateRoot} directory
File name should be in the format of {EntityName}Mapper.xml
Namespace should use the full path of the mapper interface
SQL ID should be the same as the method name
Example:

xml<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="kr.go.example.ordermanagement.infrastructure.persistence.mapper.OrderMapper">
    
    <select id="selectOrderById" parameterType="string" resultMap="orderResultMap">
        SELECT * FROM ORDERS WHERE ORDER_ID = #{orderId}
    </select>
    
    <insert id="insertOrder" parameterType="Order">
        INSERT INTO ORDERS (ORDER_ID, CUSTOMER_ID, STATUS, CREATED_DATE)
        VALUES (#{orderId}, #{customerId}, #{status}, #{createdDate})
    </insert>
    
    <!-- Other queries -->
    
</mapper>