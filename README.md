# SOLID
doc: https://toidicodedao.com/2016/05/03/series-solid-cho-thanh-nien-code-cung-single-responsibility-principle/

*S: Single Responsibility Principle
Mỗi class chỉ nên giữ 1 trách nhiệm duy nhất
Một class có quá nhiều chức năng cũng sẽ trở nên cồng kềnh và phức tạp. Trong ngành IT, requirement rất hay thay đổi, dẫn tới sự thay đổi code. Nếu một class có quá nhiều chức năng, quá cồng kềnh, việc thay đổi code sẽ rất khó khăn, mất nhiều thời gian, còn dễ gây ảnh hưởng tới các module đang hoạt động khác.

VD: 1 class Student có quá nhiều chức năng: chứa thông tin học sinh, lưu trữ thông tin vào csdl, format hiển thị thông tin học sinh.
 Khi code lớn dần lên thì class bị phình to ra. Chưa kể khi có thêm các class khác như person, teacher thì những đoạn code như lưu thông tin vào csdl, format hiển thị thông tin sẽ 
 nằm rải rác  ở nhiều class, khó sửa chữa và nâng cấp. Để giải quyết thì cần tách ra nhiều class ,mỗi class có chức năng riêng như là class lưu thông tin, class format thông tin.
 
 *I: Open/Closed Principle
 Có thể  thoải mái mở rộng 1 module nhưng hạn chế sửa đổi bên trong module đó.
 
 VD: Ta có 3 class là Square, Circle, Rectangle. Class AreaDisplay tính diện tích các hình này và in ra. Theo code cũ, để tính diện tích, ta cần dùng hàm if để check và ép kiểu object đưa vào, sau đó bắt đầu tính. Dễ thấy nếu trong tương lại ta thêm nhiều class nữa, ta phải sửa class AreaDisplay, viết thêm chừng đó hàm if nữa.
Để giải quyết vấn đề ta chuyển module tính diện tích vào mỗi class. Class AreaDisplay chỉ việc in ra. Trong tương lai, khi thêm class mới, ta chỉ việc cho class này kế thừa class Shape ban đầu. Class AreaDisplay có thể in ra diện tích của các class thêm vào mà không cần sửa gì tới source code của nó cả.

*L: Liskov Substitution Principle
Trong một chương trình, object của lớp con có thể thay thế object của lớp cha mà không làm thay đổi tính đúng đắn của chương trình

VD1: Giả sử ta muốn viết chương trình mô tả các loài chim bay. Đại bàng, chim sẻ bay được, nhưng chim cánh cụt không bay được. Do chim cánh cụt cũng là chim nên ta cho nó kể thừa lớp Bird. Tuy nhiên vì cánh cụt không biết bay nên khi gọi hàm bay của cánh cụt thì ta quăng ra Exception. 
Ta tạo 1 mảng danh sách các loài chim rồi duyệt các phần tử. Khi gọi hàm Fly thì đến chim cánh cụt chương trình sẽ gây lỗi. Lúc này chim cánh cụt đã không thay thế được class cha của nó là Bird => nó đã vi phạm LSP.

VD2: Lớp con làm thay đổi hành vi của lớp cha
Ta có 2 class cho hình vuông và hình chữ nhật. Ai cũng biết hình vuông là hình chữ nhật có 2 cạnh bằng nhau do đó ta có thể cho class Square kế thừa class Rectangle để tái sử dụng code.
  
    public class Rectangle{
  
      public int Height { 
        get;
      }
      
      public int Width { 
        get; 
      }
    
    public void SetHeight(int height)
    {
        this.Height = height;
    }
    public void SetWidth(int width)
    {
        this.Width = width;
    }
    public int CalculateArea()
    {
        return this.Height * this.Width;
    }
   }
   
   
    public class Square : Rectangle {
   
      public override void SetHeight(int height)
      {
          this.Height = height;
          this.Width = height;
      }
    
      public override void SetWidth(int width)
      {
          this.Height = width;
          this.Width = width;
      }
    }
    
    Rectangle rect1 = new Square(); 
    rect1.SetHeight(10);
    rect1.SetWidth(5);
    System.Console.WriteLine(rect1.CalculateArea()); 
    
// Kết quả là 5 x 5. Nếu đúng phải là 10×5, vì diện tích 1 hình chữ nhật là dài x rộng
// Class Square sửa hành vi của class cha Rectangle, set cả dài và rộng về 5 

*Lưu ý và kết luận
Đây là nguyên lý… dễ bị vi phạm nhất, nguyên nhân chủ yếu là do sự thiếu kinh nghiệm khi thiết kế class. Thuông thường, design các class dựa theo đời thật: hình vuông là hình chữ nhật, chim cánh cụt là chim. Tuy nhiên, không thể bê nguyên văn mối quan hệ này vào code. Hãy nhớ 1 điều:
Trong đời sống, A là B (hình vuông là hình chữ nhật, chim cánh cụt là chim) không có nghĩa là class A nên kế thừa class B. Chỉ cho class A kế thừa class B khi class A thay thế được cho class B.
Hình vuông là hình chữ nhật nhưng không thay thế được cho hình chữ nhật, chim cánh cụt là chim nhưng không thay thế được cho chim, do đó 2 ví dụ này vi phạm LSP.

*I: Interface Segregation Principle
Thay vì dùng 1 interface lớn, ta nên tách thành nhiều interface nhỏ, với nhiều mục đích cụ thể

Interface là một lớp rỗng chỉ chứa khai báo về tên phương thức không có khai báo về thuộc tính hay thứ gì khác và các phương thức này cũng là rỗng. Bởi vậy bất kỳ lớp nào sử dụng lớp interface đều phải định nghĩa các phương thức đã khai báo ở lớp interface. Điều này tương đương với việc nếu ta tạo ra 1 interface bự (hơn 100 method chẳng hạn), mỗi class sẽ phải implement toàn bộ 100 method đó, kể những method không bao giờ sử dụng đến. Nếu áp dụng ISP, ta sẽ chia interface này ra thành nhiều interface nhỏ, các class chỉ cần implement những interface có chức năng mà chúng cần, không cần phải implement những chức năng thừa nữa.

*D: Dependency Inversion Principle
Các moudle cấp cao không nên phụ thuộc vào module cấp thấp. Cả 2 nên phụ thuộc vào abstraction

Với code thông thường thì các module cấp cao sẽ gọi các module cấp thấp => Module cấp cao phụ thuộc vào module cấp thấp. Khi module cấp thấp thay đổi thì module cấp cao cũng bị thay đổi theo. Một thay đổi kéo theo hàng loạt các thay đổi, giảm khả năng bảo trì code.
Nếu tuân theo DIP, các module cấp thấp lẫn cấp cao đều phụ thuộc vào 1 interface không đổi. Ta có thể dễ dàng thay thế, sửa đổi module cấp thấp mà không ảnh hưởng gì tới module cấp cao.

