1. Aggregate
Each aggregate is organized as a separate package
Aggregate Root class names are used as-is without adding 'Root' at the end
Example: Order, Product, Customer
Example:
```
@Entity
@Table(name="Order_table")
@Data

//<<< DDD / Aggregate Root
public class Order  {

    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;    
    
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> orderItems = new java.util.ArrayList<>();

    public void addOrderItem(String productName, Integer qty) {
        OrderItem orderItem = new OrderItem(productName, qty, this);
        orderItems.add(orderItem);
    }

    public List<OrderItem> getOrderItems() {
        return Collections.unmodifiableList(orderItems);
    }

    public void updateOrderItem(Long id, String productName, Integer qty) {
        for (OrderItem item : orderItems) {
            if (item.getId().equals(id)) {
                item.update(productName, qty);
                break;
            }
        }
    }

    public void removeOrderItem(OrderItem orderItem) {
        orderItems.remove(orderItem);
    }

    public static OrderRepository repository(){
        OrderRepository orderRepository = OrderApplication.applicationContext.getBean(OrderRepository.class);
        return orderRepository;
    }

    public void modifyOrder(ModifyOrderCommand modifyOrderCommand){
        
        //implement business logic here:
        


        OrderModified orderModified = new OrderModified(this);
        orderModified.publishAfterCommit();
    }
}
```
---

2. Entity

All entities are located in the domain/{aggregate}/entity package
Identifiers (ID) must be included, with naming convention {EntityName}Id
equals() and hashCode() must be implemented based on the identifier
Entity classes should be designed as immutable when possible
Example:

```
public class OrderItem {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String productName;

    private Integer qty;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id")
    private Order order;

    protected OrderItem() {}

    protected OrderItem(String productName, Integer qty, Order order) {
        this.productName = productName;
        this.qty = qty;
        this.order = order;
    }

    public String getProductName() {
        return productName;
    }

    public Integer getQty() {
        return qty;
    }
}
```
---
3. Value Object

All value objects are located in the domain/{aggregate}/vo package
Value objects are implemented as immutable
equals() and hashCode() are implemented based on all attributes
Value object class names are specified as nouns with clear meaning
Example:

```
@Embeddable
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Address {

    private String state;

    private String city;

    private String distict;

    private String street;
}
```
---
4. Domain Event

All domain events are located in the domain/{aggregate}/event package
Event class names start with a past tense verb and end with Event
Example: OrderPlacedEvent, PaymentCompletedEvent
Events are implemented as immutable
Events must include occurrence time
Example:

```
@Data
@ToString
public class OrderModified extends AbstractEvent {

    private Long id;
    private String userId;
    private Date orderDate;
    private Address address;
    private OrderStatus orderStatus;
    private List<OrderItem> orderItems;

    public OrderModified(Order aggregate) {
        super(aggregate);
    }

    public OrderModified() {
        super();
    }
}
```
---
5. Enum
Example:
```
public enum OrderStatus {
    ORDERPLACED,
    ORDERCANCELED,
}
```
---
6. Repository Interface

All repository interfaces are located in the domain/{aggregate}/repository package
Interface names follow the format {EntityName}Repository
Repositories are defined at the aggregate level
Example:
```
@RepositoryRestResource(collectionResourceRel = "orders", path = "orders")
public interface OrderRepository
    extends PagingAndSortingRepository<Order, Long> {}

```
---
7. Controller

Declare APIs for basic CRUD operations and extended APIs for Commands with isRestRepository set to false in the metadata, and process them to call the methods of the service.
Example:
```
@RestController
// @RequestMapping(value="/orders")
public class OrderController {

    @Resource(name = "orderService")
    private OrderService orderService;

    @GetMapping("/orders")
    public List<Order> getAllOrders() throws Exception {
        // Get all orders via OrderService
        return orderService.getAllOrders();
    }

    @GetMapping("/orders/{id}")
    public Optional<Order> getOrderById(@PathVariable Long id)
        throws Exception {
        // Get a order by ID via OrderService
        return orderService.getOrderById(id);
    }

    @PostMapping("/orders")
    public Order createOrder(@RequestBody Order order) throws Exception {
        // Create a new order via OrderService
        return orderService.createOrder(order);
    }

    @PutMapping("/orders/{id}")
    public Order updateOrder(@PathVariable Long id, @RequestBody Order order)
        throws Exception {
        // Update an existing order via OrderService
        return orderService.updateOrder(order);
    }

    @DeleteMapping("/orders/{id}")
    public void deleteOrder(@PathVariable Long id) throws Exception {
        // Delete a order via OrderService
        orderService.deleteOrder(id);
    }

    @RequestMapping(
        value = "/orders/{id}/modifyorder",
        method = RequestMethod.PUT,
        produces = "application/json;charset=UTF-8"
    )
    public Order modifyOrder(
        @PathVariable(value = "id") Long id,
        @RequestBody ModifyOrderCommand modifyOrderCommand,
        HttpServletRequest request,
        HttpServletResponse response
    ) throws Exception {
        return orderService.modifyOrder(modifyOrderCommand);
    }
}
```