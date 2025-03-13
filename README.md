using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

public class order
{
    public int Id { get; set; }
    public string Name { get; set; }
    public List<string> items { get; set; }
    public decimal price { get; set; }
    public order(int id, string name, List<string> items, decimal price)
    {
        Id = id;
        Name = name;
        decimal priceString = price;
        items = items;
    }

    // ocp
    public interface Ipayment
    {
        void ProcessPayment(decimal price);
    }
    public class Easypaisa : Ipayment
    {
        public void ProcessPayment(decimal price)
        {
            Console.WriteLine($"Processing EasyPaisa payment of amount:{price}");
        }

        public class Nayapaisa : Ipayment
        {
            public void ProcessPayment(decimal price)
            {
                Console.WriteLine($"Processing NayaPaisa payment of amount:{price}");
            }
        }
    }
    // Liskov Substitution Principle (LSP)
    // OrderProcessor does not depend on specific payment methods
    public class OrderProcessor
    {
        private readonly Ipayment _paymentProcessor; 
public OrderProcessor(Ipayment paymentProcessor)
        {
            _paymentProcessor = paymentProcessor;
        }
        public void ProcessOrder(order order)
        {
            Console.WriteLine($"Processing Order #{order.Id} for {order.Name}");
            Console.WriteLine($"Total Amount: {order.price}");
            _paymentProcessor.ProcessPayment(order.price);
            Console.WriteLine("Order processed successfully!\n");
        }
    }
    public interface IShippingService
    {
        void ShipOrder(order order);
    }

    // Implementation of standard shipping service
    public class StandardShipping : IShippingService
    {
        public void ShipOrder(order order)
        {
            Console.WriteLine($"Shipping Order #{order.Id} via Standard Shipping");
        }
    }

    // Implementation of express shipping service
    public class ExpressShipping : IShippingService
    {
        public void ShipOrder(order order)
        {
            Console.WriteLine($"Shipping Order #{order.Id} via Express Shipping");
        }
    }
    public class OrderService
    {
        private readonly IShippingService _shippingService; // Shipping service abstraction

        public OrderService(IShippingService shippingService)
        {
            _shippingService = shippingService;
        }

        public void Ship(order order)
        {
            _shippingService.ShipOrder(order);
        }
    }

    // Program Execution (Main Method)
    class Program
    {
        static void Main(string[] args)
        {
            
            List<string> items = new List<string> { "Laptop", "Mouse", "Keyboard" };
            order order1 = new order(101, "Ali", items, 124657);

            // Select a payment method (easy paisa)
            Ipayment paymentProcessor = new Easypaisa();

            // Process the order
            OrderProcessor orderProcessor = new OrderProcessor(paymentProcessor);
            orderProcessor.ProcessOrder(order1);

            // Select a shipping method (Express Shipping)
            IShippingService shippingService = new ExpressShipping();

            // Ship the order
            OrderService orderService = new OrderService(shippingService);
            orderService.Ship(order1);
        }
    }
}




in this class we learn about design principle solid or many more principle 
