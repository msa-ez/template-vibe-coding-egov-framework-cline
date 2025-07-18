Service Layer (Business Layer)

1. Service Interface
package sample.service;

public interface OrderService {
    /**
     * Order list lookup    
     * @param orderVO
     * @return
     * @throws Exception
     */
    List<OrderVO> selectOrderList(OrderVO orderVO) throws Exception;
    
    /**
     * Order detail lookup
     * @param orderId
     * @return
     * @throws Exception
     */
    OrderVO selectOrder(String orderId) throws Exception;
    
    /**
     * Order registration
     * @param orderVO
     * @return
     * @throws Exception
     */
    String insertOrder(OrderVO orderVO) throws Exception;
    
    /**
     * Order modification
     * @param orderVO
     * @return
     * @throws Exception
     */
    int updateOrder(OrderVO orderVO) throws Exception;
    
    /**
     * Order deletion
     * @param orderId
     * @return
     * @throws Exception
     */
    int deleteOrder(String orderId) throws Exception;
}

2. Service Implementation
package sample.service.impl;

import java.util.List;
import javax.annotation.Resource;

import org.springframework.stereotype.Service;
import egovframework.rte.fdl.cmmn.EgovAbstractServiceImpl;


@Service("orderService")
public class OrderServiceImpl extends EgovAbstractServiceImpl implements OrderService {
    
    @Resource(name = "orderDAO")
    private OrderDAO orderDAO;
    
    /**
     * Order list lookup
     */
    @Override
    public List<OrderVO> selectOrderList(OrderVO orderVO) throws Exception {
        return orderDAO.selectOrderList(orderVO);
    }
    
    /**
     * Order detail lookup
     */
    @Override
    public OrderVO selectOrder(String orderId) throws Exception {
        return orderDAO.selectOrder(orderId);
    }
    
    /**
     * Order registration
     */
    @Override
    public String insertOrder(OrderVO orderVO) throws Exception {
        // Business logic processing (e.g., order number generation, inventory check, etc.)
        return orderDAO.insertOrder(orderVO);
    }
    
    /**
     * Order modification
     */
    @Override
    public int updateOrder(OrderVO orderVO) throws Exception {
        // Business logic processing (e.g., additional tasks based on status changes)
        return orderDAO.updateOrder(orderVO);
    }
    
    /**
     * Order deletion
     */
    @Override
    public int deleteOrder(String orderId) throws Exception {
        return orderDAO.deleteOrder(orderId);
    }
}

---

DAO Layer (Data Access Layer)
 
1. DAO Interface
package sample.dao;

import java.util.List;

public interface OrderDAO {
    /**
     * Order list lookup
     * @param orderVO
     * @return
     * @throws Exception
     */
    List<OrderVO> selectOrderList(OrderVO orderVO) throws Exception;
    
    /**
     * Order detail lookup
     * @param orderId
     * @return
     * @throws Exception
     */
    OrderVO selectOrder(String orderId) throws Exception;
    
    /**
     * Order registration
     * @param orderVO
     * @return
     * @throws Exception
     */
    String insertOrder(OrderVO orderVO) throws Exception;
    
    /**
     * Order modification
     * @param orderVO
     * @return
     * @throws Exception
     */
    int updateOrder(OrderVO orderVO) throws Exception;
    
    /**
     * Order deletion
     * @param orderId
     * @return
     * @throws Exception
     */
    int deleteOrder(String orderId) throws Exception;
}

2. DAO Implementation with MyBatis
package sample.dao.impl;

import java.util.List;

import org.springframework.stereotype.Repository;
import egovframework.rte.psl.dataaccess.EgovAbstractMapper;

@Repository("orderDAO")
public class OrderDAOImpl extends EgovAbstractMapper implements OrderDAO {
    
    /**
     * Order list lookup
     */
    @Override
    public List<OrderVO> selectOrderList(OrderVO orderVO) throws Exception {
        return selectList("Order.selectOrderList", orderVO);
    }
    
    /**
     * Order detail lookup
     */
    @Override
    public OrderVO selectOrder(String orderId) throws Exception {
        return selectOne("Order.selectOrder", orderId);
    }
    
    /**
     * Order registration
     */
    @Override
    public String insertOrder(OrderVO orderVO) throws Exception {
        insert("Order.insertOrder", orderVO);
        return orderVO.getOrderId();
    }
    
    /**
     * Order modification
     */
    @Override
    public int updateOrder(OrderVO orderVO) throws Exception {
        return update("Order.updateOrder", orderVO);
    }
    
    /**
     * Order deletion
     */
    @Override
    public int deleteOrder(String orderId) throws Exception {
        return delete("Order.deleteOrder", orderId);
    }
}
