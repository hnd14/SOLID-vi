# Nguyên Lý SOLID
## AGILE SD PRINCIPLES, PATTERNS, and PRACTICES by Robert C. Martin

## S - Single Responsibility Principle (SRP)

Nguyên lý này được diễn tả bởi Tom DeMarco và Meilir Page-Jones bằng từ *cohesion* có nghĩa là sự liên quan về mặt chức năng giữa các thành phần trong một module. Trong phần này, chúng ta sẽ thay đổi ý nghĩa này một chút và liên hệ *cohesion* với *nguyên nhân thay đổi* của một module hoặc một class.

### Phát biểu SRP:
> A class should have only one reason to change.

Tạm dịch

> Một lớp chỉ nên có duy nhất một lý do để thay đổi.

Xét đến game Bowling ở Chương 6. Trong hầu hết quá trình phát triền, lớp `Game` đã xử lý hai trách nhiệm khác nhau bao gồm kiểm soát trạng thái hiện tại của trò chơi và tính điểm cho trò chơi. Cuối cung RCM và RSK đã tách 2 nhiệm vụ ra cho 2 lớp khác nhau, lớp `Game` vẫn giữ nhiệm vụ kiểm soát trạng thái của trò chơi trong khi một lớp `Scorer` mới lãnh nhiệm vụ tính điểm.

Tại sao việc tách hai nhiệm vụ kể trên ra cho 2 lớp khác nhau lại quan trọng? Đó là bởi vì mỗi nhiệm vụ thể hiện một hướng mà sản phẩm có thể thay đổi. Khi yêu cầu thay đổi, những thay đổi đó sẽ được thể hiện thông qua sự thay đổi nhiệm vụ của các lớp. Nếu một lớp chịu trách nhiệm cho nhiều hơn một nhiệm vụ, sẽ có nhiều hơn một kiểu thay đổi có thể xảy ra cho lớp đó.

Khi một lớp mang nhiều nhiệm vụ, những nhiệm vụ đó sẽ trờ nên phụ thuộc (*coupling*) vào nhau. Sự thay đổi của về yêu cầu của một 
nhiệm vụ khi ấy sẽ gây cản trở đếm việc lớp đó hoàn thành những nhiệm vụ còn lại, dẫn đến cấu trúc của phần mềm trở nên dễ vỡ theo những hường không thể lường trước trước những thay đổi.

Xét ví dụ trong hình 8.1 dưới đây. Lớp `Rectangle` có 2 phương thức là `draw()` để vẽ hình chữ nhật lên `GUI` và `area()` để tính diện tích hình chữ nhật.

![Figure 8.1](./imgs/Figure%208-1.png)

***Hình 8-1.** Nhiều hơn 1 nhiệm vụ*

Có 2 loại ứng dụng sử dụng lớp `Rectangle` bao gồm một ứng dụng tính toán hình học (`Computational Geometry Application`) và một ứng dụng đồ họa (`Graphical Application`). Ứng tính toán sẽ chỉ tính diện tích bằng phương thức `area()` không bao giờ vẽ bằng `draw()`. Ứng dụng đồ họa thì chắc chắn sẽ vẽ hình chữ nhật với `draw()`, và nó cũng có thể cần tính diện tích với `area()`.

Thiết kế trên đây vi phạm **SRP** vì lớp `Rectangle` đảm nhận 2 nhiệm vụ bao gồm tính toán diện tích và vẽ hình chữ nhật lên màn hình. Việc này dẫn đến một vài hệ lụy xấu. Đầu tiên chúng ta sẽ cần phải mang lớp `GUI` vào trong phần mềm tính toán hình học để sử dụng lớp `Rectangle` mặc dù điều này là hoàn toàn không cần thiết. Trong C++, điều này nghĩa là `GUI` phải được liên kết, làm tốn thời gian liên kết, dịch mã cũng như làm tăng việc tiêu thụ bộ nhớ. Trong Java, việc này có nghĩa là chúng ta phải mang file `.class` của `GUI` theo khi deploy phần mềm tính toán.

Hơn thế nữa, việc thiết kế này vi phạm `SRP` còn gây hại khi có những thay đổi xảy ra với `Graphical Application` dẫn đến việc cần thay đổi lớp `Rectangle`. Khi đó, chúng ta sẽ cần phải compile, build và deploy lại cả `Computational Geometry Application` mặc dù không có bất kỳ thay đổi nào về tính năng hay yêu cầu của ứng dụng này nếu không muốn gặp phải những lỗi không lường trước được. 

Một thiết kế tốt hơn cho trường hợp này có thể đạt được bằng cách tách 2 nhiệm vụ ra cho 2 lớp tách biệt như ở hình 8.2. Ở thiết kế này, chức năng tính toán diện tích hình chữ nhật được tách riêng cho lớp `Geometric Rectange`. Bằng cách này, những thay đổi liên quan đến đồ họa của hình chữ nhật sẽ không thể gây ảnh hưởng đến việc tính toán.

![Figure 8-2](./imgs/Figure%208-2.png)

***Hình 8-1.** SRP*

### Vậy thế nào là 1 nhiệm vụ?

Trong **SRP**, chúng ta nói rất nhiều về nhiệm vụ, và nó nên được định nghĩa là **hướng thay đổi**. Nếu bạn có thể nghĩ đến nhiều hơn một cách mà một lớp có thể thay đổi, thì lớp đó đang mang nhiều hơn một nhiệm vụ. Điều này đôi khi có thể khó nhận ra vì chúng ta thường nghĩ đến nhiệm vụ như là trách nhiệm của mỗi người trong một tập thể. Ví dụ như interface `Modem` trong snippet dưới đây. Hầu hết chúng ta đều đồng ý rằng interface này trông hợp lý vì đầy đều là những tính năng thường có của một `Modem`.

```Java
interface Modem {
    public void dial(String pno);
    public void hangup();
    public void send(char c);
    public char recv();
}
```

Tuy vậy, interface này đang vi phạm **SRP** vì có 2 nhiệm vụ đang được quản lý ở đây. Đầu tiên là việc quản lý các kết nối với `dial` và `hangup`, sau đó là việc gửi và nhận data với `send` và `recv`.

Vậy 2 trách nhiệm này có nên được tách ra? Điều này còn tùy thuộc vào những hướng thay đổi tiềm tàng của ứng dụng này. Nếu ứng dụng này nhận những thay đổi thường xuyên về một trong 2 tính năng này trong khi không có thay đổi về tính năng còn lại thì đây sẽ là một dấu hiệu cho thấy thiết kế này bị cứng nhắc (**Rigidity**) khi ta sẽ phải compile và deploy lại nó thường xuyên hơn ta mong muốn.

![Figure 8-3](./imgs/Figure%208-3.png)

***Hình 8-1.** Modem interface được phân rã*

Ngược lại, nếu ứng dụng dùng design này không phải nhận những thay đổi về 2 nhiệm vụ này nhiều, hoặc thay đổi của 2 nhiệm vụ này luôn đến một cách cùng lúc thì việc tách interface này ra làm 2 là không cần thiết và là dấu hiệu của việc phức tạp hóa không cần thiết (**Needless complexity**).

Một hệ quả rút ra ở đây là một **hướng thay đổi** chỉ xác định khi và chỉ khi những thay đổi theo hướng đó diễn ra.

### Tách rời các nhiệm vụ
Chú ý rằng trong hình 8-3, 2 nhiệm vụ kết nối và truyền tải thông tin vẫn bị ràng buộc trong class `ModelImpl`. Đây không phải là điều chúng ta mong muốn, tuy nhiên đôi khi chúng ta bị buộc phải làm như vậy vì những ràng buộc ở hệ điều hành hay phần cứng. Tuy nhiên, việc tạo ra 2 interface riêng biệt giúp  tách rời 2 nhiệm vụ này về mặt logic tại phần còn lại của phần mềm.

Mặc dù `ModemImpl` vi phạm **SRP**, chú ý rằng không có bất kỳ thành phần nào phụ thuộc trực tiếp vào nó. Không bất kỳ class nào ngoài `Main` cần phải biết đến sự tồn tại của `ModelImpl`. Bằng cách này, ta đã che giấu được sự bất cập của `ModelImpl` khỏi phần còn lại của phần mềm. 
### Persistence
Hình 8-4 là một ví dụ thường gặp về việc vi phạm **SRP** thường gặp. Lớp `Employee` chứa cả các business rules và quản lý persistence. Hai nhiệm vụ này nên được phân rã ra vì các business rules sẽ thường xuyên thay đổi trong khi việc quản lý persistence thì sẽ hiếm khi thay đổi, và kể cả khi thay đổi, những thay đổi liên quan đển persistence và các thay đổi về business rules sẽ không ảnh hưởng đến nhau.

![Figure 8-4](./imgs/Figure%208-4.png)

***Hình 8-1.** Ràng buộc persistence*

May mắn thay, test driven development (TDD) thường sẽ bắt buộc sự tách bạch này từ lâu trước khi design trở nên bất ổn. Tuy vậy, trong những trường hợp mà TDD không bắt buộc sự phân rã xảy ra, thiết kế này vẫn nên được refactor với **FACADE** hoặc **PROXY** pattern.

### Kết luận
SRP là một trong những nguyên lý đơn giản nhất, nhưng đồng thời cũng là nguyên lý khó nhất để hiểu và áp dụng đúng. Xử lý nhiều nhiệm vụ là điều mà chúng ta làm một cách tự nhiên, và việc tìm và phân rã các nhiệm vụ này chiếm phần lớn thời gian của công việc thiết kế phần mềm. Trong những nguyên lý tiếp theo, chúng ta cũng sẽ luôn phải liên tục nhắc lại về **SRP**.

## O - Open-Closed Principle (OCP)
Ivar Jacob từng nói: "Tất cả mọi hệ thống đều thay đổi trong vòng đời của nó. Đây là điều cần được ghi nhớ nếu muốn hệ thống được thiết kế có thể trụ được dài hơn một phiên bản." Vậy làm sao có thể thiết kế một hệ thống có thể kéo dài hơn 1 phiên bản? Nguyên lý đóng mở (OCP) được Bertrand Meyer phát biểu vào năm 1988 cho ta những định hướng để làm được việc đó.

### Phát biểu OCP:
> Software entities (classes, modules, functions,...) should be open for extension, but closed for modification.

Tạm dịch 
> Các thành phần của phần mềm (lớp, module, hàm,...) nên được mở cho mở rộng, nhưng đóng cho chỉnh sửa.

Khi một thay đổi duy nhất của phần mềm dẫn đến một loạt những thay đổi đi theo, đó là dấu hiệu cho thấy thiết kế hiện tại của phần mềm đang bị cứng nhắc (**Rigidity**). OCP khuyên chúng ta nên refactor phần mềm để những thay đổi tương tự sẽ không kéo theo những thay đổi khác nữa. Nếu OCP được áp dụng tốt, những thay đổi của hệ thống sẽ được tạo ra bằng cách thêm code mới chứ không phải chỉnh sửa code hiện tại.

### Chú giải
Một module tuân thủ OCP sẽ có 2 tính chất chính:

1. Mở cho mở rộng.
Điều này có nghĩa là các hành vi của module có thể được mở rộng. Khi yêu cầu thay đổi, ta có thể mở rộng module với những hành vi mới để đáp ứng yêu cầu mới. Nói cách khác, ta có thể thay đổi việc mà module thực hiện
2. Đóng cho chỉnh sửa
Chỉnh sửa về hành vi của module không tạo ra thay đổi của source hay binary code của module. 

Hai điều trên đây trông có vẻ mâu thuẫn. Những thay đổi về hành vi của module thông thường sẽ đạt được thông qua sự thay đổi source code của module. Ngược lại, một module mà source và binary code không đổi sẽ làm ta nghĩ là hành động của nó không đổi. Vậy làm sao để ta đạt được cả 2 điều trên một cách đồng thời? Làm sao để thay đổi hành vi của module mà không làm thay đổi source code của nó?

### Tính trừu tượng chính là chìa khóa

Trong C++, Java, hay bất kì ngôn ngữ OOP nào khác, ta có thể tạo ra các lớp trừu tượng cố định nhưng có thể biểu diễn một lượng không giới hạn các hành vi khác nhau. Những hành vi này sẽ được biểu diễn thông qua những lớp kế thừa của lớp trừu tượng này.

Một module có thể tương tác với một lớp trừu tượng cố định có thể được mở rộng. Bằng cách này, module đó có thể được đóng với chỉnh sửa nhưng vẫn có thể thay đổi hành vi nhờ vào những lớp con mở rộng từ lớp trừu tượng.

Hình 9-1 thể hiện một design đơn giản vi phạm OCP. Cả 2 lớp lớp Client và Server đều là các lớp cụ thể. Nếu ta muốn client sử dụng một Server khác, ta sẽ phải được chỉnh sửa để sử dụng một Server khác.

![Figure 9-1](./imgs/Figure%209-1.png)

***Hình 9-1.** Client không tuân thủ OCP*

Hình 9-2 biểu diễn một design khác tuân thủ OCP. Trong trường hợp này `ClientInterface` là một lớp trừu tượng với các phương thức trừu tượng. Lớp `Client` sẽ dùng lớp trừu này, nhưng các object của lớp `Client` sẽ dùng các object thuộc các lớp cụ thể mở rộng từ  `ClientInterface`. Nếu ta muốn một `Client` sử dụng một server khác, ta có thể tạo một lớp `Server` khác mở rộng từ `ClientInterface` và giữ nguyên lớp `Client` như cũ.

Lớp `Client` có những hành động nó cần được thực hiện để hoàn thành nhiệm vụ của mình. Nó sẽ diễn tả các hành động này thông qua các phương thức trừu tượng của lớp `ClientInterface`. Các lớp con của `ClientInterface` có thể hiện thực hóa các phương thức của `ClientInterface` theo bất kỳ cách nào phù hợp. Bằng cách này, các hoạt động của lớp `Client` có thể được thay đổi bằng cách tạo ra các lớp con của `ClientInterface`.

![Figure 9-2](./imgs/Figure%209-2.png)

***Hình 9-2.**`Strategy` pattern, `Client` tuân thủ OCP*

Có thể bạn sẽ thắc mắc tại sao lớp trừu tượng này lại được gọi là `ClientInterface` thay vì `AbstractServer`. Lý do cho việc này, mà chúng ta sẽ cùng xem xét sau này, là vì những lớp trừu tượng liên kết chặt chẽ với các lớp sử dụng chúng hơn là những lớp cụ thể hóa chúng.

Một cách khác thường dùng để đạt được OCP được mô tả ở hình 9-3. Lớp `Policy` có một vài hàm `public` được viết để xử lý một vài công việc xác định. Tuy vậy trong trường hợp này, interface trừu tượng lại trở thành 1 phần của lớp `Policy` (hàm ServiceFunction()). Bằng cách này, các lớp cụ thể hóa `Policy` có thể thay đổi hành vi của class `Policy` thông qua việc thay đổi implement `ServiceFunction()` mà không cần thay đổi source code của `Policy`.

![Figure 9-3](./imgs/Figure%209-3.png)

***Hình 9-3.** `Template method` pattern, `Policy` tuân thủ OCP*

# L - Liskov Substitution Principles (LSP)

Nguyên lý chính để đạt được OCP là tận dụng tính trừu tượng(abstraction) và tính đa hình(Polymorphism). Trong các ngôn ngữ lập trình `Statically typed` như Java hay C++, tính kế thừa(inheirantance) chính là phương pháp chính để đạt được tính trừu tượng và đa hình. Bằng cách dùng sự kế thừa, chứng ta có thể tạo ra những lớp con cụ thể hóa những hàm và lớp trừu tượng ban đầu.

Vậy làm sao để tận dụng tốt tính kế thừa? Làm sao để nhận biết một hệ thống kế thừa tốt? Đâu là những sai lầm thường gặp trong việc thiết kế một hệ thống kế thừa làm nó vi phạm OCP? Những điều này sẽ được làm rõ với LSP.

### Phát biểu LSP
> SUBTYPES MUST BE SUBTITUABLE FOR THEIR BASE TYPES.

Tạm dịch:

> Các lớp con phải có thể thay thế cho lớp cha của chúng.

Barara Liskov phát biểu nguyên lý này lần đầu vào năm 1988 như sau:


> Điều mà ta mong muốn đạt được ở đây như sau: Với mỗi vật o<sub>1</sub> thuộc lớp S, tồn tại một vật thuộc lớp o<sub>2</sub> T sao cho với tất cả mọi phần mềm P sử dụng T, hành vi của P không thay đổi khi ta thay o<sub>1</sub> với o<sub>2</sub>. Khi đó, ta nó S là một subtype của T.

Sự quan trọng của nguyên lý này trở nên rõ ràng khi ta xem xét chuyện gì sẽ xảy ra khi nguyên lý này bị vi phạm. Giả sử chúng ta có một hàm *f* nhận vào một reference đến một lớp B. Giả sử có một lớp D mở rộng từ lớp B mà khi một reference đến D được đưa vào hàm *f* làm cho nó hoạt động sai. Khi đó, ta có thể thấy rằng lớp D đang vị phạm LSP, và có thể thấy rằng D trở nên dễ vỡ với sự hiện diện của *f*.

Có người sẽ vội vàng sửa thêm một số test vào hàm *f* để *f* hoạt động bình thường khi D được đưa vào. Tuy nhiên việc làm như vậy lại vi phạm OCP khi hàm *f* phải bị chỉnh sửa với mỗi lớp mở rộng từ B. Đây là phản ứng thường gặp của các lập trình viên thiếu kinh nghiệm(hoặc tệ hơn, cấc lập trình viên vội vàng) khi vi phạm LSP diễn ra.

### Một ví dụ nhỏ về việc vi phạm LSP

Việc vi phạm LSP thường dẫn đến việc sử dụng những thông tin ở run-time (Run-time type information hay RTTI). Thường thấy nhất là khi những câu lệnh `if` thực hiện việc kiểm tra kiểu tường minh cho một vật để xác định cách xử lý cho chúng.

```cpp
struct Point{double x,y;};

struct Shape{
    enum ShapeType {square, circle} itsType;
    Shape(ShapeType t): itsType(t) {}
};

struct Circle{
    Circle() : Shape(circle) {};
    void Draw() const;
    Point itsCenter;
    double its Radius;
};

struct Square{
    Square() : Shape(square) {};
    void Draw() const;
    Point itsCenter;
    double its Radius;
};

void DrawShape(const Shape& s)
{
    if (s.itsType == Shape::square)
        static_cast<const Square&>(s).draw()
    if (s.itsType == Shape::circle)
        static_cast<const Circle&>(s).draw()

}
```

***Snippet 10-1***

Dễ nhận thấy hàm `DrawShape()` trong snippet 10-1 ở trên đang vi phạm OCP. Nó cần phải biết tất cả các lớp được mở rộng ra từ lớp `Shape`, cũng như nó sẽ phải được chỉnh sửa mỗi khi có một lớp mới được mở rộng từ `Shape`. Dễ nhận thấy rằng đây là một thiết kế không tốt. Điều gì làm cho một lập trình viên thiết kế code như vậy?

Xét trường hợp Kỹ Sư Joe. Joe đã nghiên cứu về OOP và kết luận rằng chi phí cho việc xử lý đa hình là quá nhiều. Vì vậy, anh ấy quyết định định nghĩa lớp `Shape` mà không dùng bất kỳ phương thức trừu tượng nào. 2 lớp `Square` và `Circle` mở rộng từ `Shape` có hàm `draw()`, nhưng vì chúng không override bất kỳ hàm nào ở Shape, `DrawShape` phải kiểm tra kiểu của từng input và gọi đúng hàm `draw()` cho từng trường hợp.

Việc `Square` và `Circle` không thể thay thế được bằng `Shape` đã vi phạm LSP và dẫn đén việc hàm `DrawShape()` phải vi phạm OCP. Có thể nói rằng việc vi phạm `LSP` sẽ dẫn đến nguy cơ vi phạm `OCP` một cách tiềm tàng.

### `Square` và `Rectangle`, một sự vi phạm khó nhận ra hơn

```cpp
class Rectangle 
{
    public:
        void setWidth(double w){itsWidth=w}
        void setHeight(double h){itsHeight=h}
        double getWidth() const {return itsWidth;}
        double getHeight() const {return itsWidth;}
    private:
        Point itsTopLeft;
        double itsWidth;
        double itsHeight;
};
```

Giả sử rằng ứng dụng hoạt động tốt và được sử dụng ở nhiều nơi. Và vì vậy, người dùng sẽ thi thoảng yêu cầu thay đổi nó. Một ngày nọ, một người dùng muốn ứng dụng này có thể xử lý cả hình vuông thay vì hình chữ nhật.

Ta thường nói tình kế thừa thể hiện một mối quan hệ "Là một". Nói cách khác, nếu một lớp mới thỏa mãn việc này với một lớp có sẵn, nó nên được thừa kế từ lớp đó.

Ta có thể nói rằng một Hình vuông là một Hình chữ nhật trong hầu hết mọi ứng dụng và trường hợp. Vì vậy, sẽ rất hợp lý nếu để hình vuông kế thừa từ hình chữ nhật.

![Figure 10-1](./imgs/Figure%2010-1.png)

***Hình 10-1.** Hình vuông kế thừa hình chữ nhật*

Vì hình vuông là hình chữ nhật nên lớp hình vuông nên được kế thừa từ lớp hình chữ nhật.

Tuy vậy, việc này có thể dẫn đến những vấn đề khó nhận thấy nếu ta không bắt tay vào code.

Dấu hiệu đầu tiên cho ta thấy rằng có vấn đề đối với thiết kế này là việc hình vuông không cần thiết có cả chiều dài và chiều rộng riêng biệt. Tuy vậy, `Square` vẫn sẽ thừa kế những field này từ `Rectangle`. Việc này rõ ràng là thừa thải. Trong nhiều phần mềm, việc thừa thải này là không đáng kể. Tuy nhiên, nếu ta cần phải tạo hàng trăm nghìn `Square` (ví dụ như trong các phần mêm CAD/CAE), việc này có thể gây ảnh hưởng lớn đền hoạt động của phần mềm.

Kể cả khi việc tối ưu hóa RAM không phải ưu tiên của dự án này vẫn có những vấn đề khác xảy đến từ việc để `Square` kế thừa từ `Rectangle`. Khi này `Square` sẽ thừa kế 2 phương thức `setHeight` và `setWidth` từ `Rectangle`, và 2 phương thức này không phù hợp với `Square` vì cả chiều dài và chiều rộng của hình vuông phải bằng nhau. Đây là một dấu hiệu rõ ràng cho thấy rằng có vấn đề với thiết kế hiện tại của phần mềm. Tuy vậy, vẫn có thể né tránh vấn đề này bằng cách override 2 hàm này như sau:

```cpp
void Square::setWidth(double w)
{
    Rectangle::setWidth(w);
    Rectangle::setHeight(w);
}

void Square::setHeight(double w)
{
    Rectangle::setWidth(w);
    Rectangle::setHeight(w);
}
```

Bây giờ, nếu có một người thay đổi chiều dài của hình vuông, chiều rộng của nó cũng sẽ thay đổi theo và ngược lại. Bằng cách này, các tính chất của hình vuông vẫn luôn được đảm bảo

```cpp
Square s
s.setWidth(1) // chỉnh sửa cả chiều dài về 1 
s.setHeight(2) // chỉnh luôn chiều rộng về 2
```

Nhưng trong trường hợp này:

```cpp
void f(Rectangle& r)
{
    f.setWidth(32) // dùng setWidth của Rectangle 
}
```

Nếu ta cho một hình vuông vào hàm *f*, hình vuông đó sẽ bị hư hại khi chỉ `width` được set về 32 trong khi `height` thì vẫn giữ nguyên. Đây rõ ràng là một sai phạm về LSP. Hàm *f* không hoạt động cho `Square` làm một class con của `Rectangle`. Đó là vì hàm `setHeight` và `setWidth` không được khai báo với `virtual` nên chúng không mang tính đa hình.

Ta có thể sửa điều này một cách đơn giản. Tuy nhiên việc tạo ra một lớp mới buộc ta phải thực hiện chỉnh sửa lớp đã có là một dấu hiệu cho thấy kiến trúc của ta có vấn đề khi ta buộc phải vi phạm OCP. Cũng có ý kiến cho rằng việc ta không khai báo `virtual` cho `setHeight` và `setWidth` chính là vấn đề thực sự của thiết kế hiện tại. Tuy vậy, ý kiến này không thực sự thuyết phục khi mà đây là những phương thức vô cùng cơ bản. Vì lý do gì mà ta phải khai báo chúng là `virtual` nếu ta không đoán trước việc phải xử lý hình vuông.

Tuy vậy, cứ thử xem xét lập luận đó và thử sửa lại code. Ta sẽ được đoạn code ở snipper 10-3

```cpp
class Rectangle 
{
    public:
        virtual void setWidth(double w){itsWidth=w}
        virtual void setHeight(double h){itsHeight=h}
        double getWidth() const {return itsWidth;}
        double getHeight() const {return itsWidth;}
    private:
        Point itsTopLeft;
        double itsWidth;
        double itsHeight;
};

class Square : public Rectangle {
    virtual void setWidth(double w){itsWidth=w}
    virtual void setHeight(double h){itsHeight=h}    
}

void Square::setWidth(double w)
{
    Rectangle::setWidth(w);
    Rectangle::setHeight(w);
}

void Square::setHeight(double w)
{
    Rectangle::setWidth(w);
    Rectangle::setHeight(w);
}
```

### Vấn đề thực sự

`Square` và `Rectangle` bây giờ trông có vẻ ổn và không còn vấn đề gì. Bất kể bạn làm gì với `Square`, nó vẫn sẽ là một hình vuông về mặt toán học. Bất kể bạn làm gì với `Rectangle`, nó vẫn sẽ giữ các tính chất của hình chữ nhật. Hơn thế nữa, ta có thể đặt một hình vuông vào một function cần hình chữ nhật và hình vuông vẫn sẽ giữ các tính chất của nó. Vậy thiết kế này đã tự nhất quán với chính nó.

Tuy vậy, thiết kế này vẫn tiềm ẩn những vấn đề. Một thiết kế tự nhất quán chưa chắc đã nhất quán đối với tất cả những client của nó. Xét hàm *g* dưới đây.

```cpp
void g(Rectangle& r)
{
    r.setWidth(4);
    r.setHeight(5);
    assert(r.Area() == 20);
}
```
Hàm này gọi `setWidth` và `setHeight` của một vật mà nó tin là hình chữ nhật. Và nó hoạt động hoàn toàn ổn với lớp `Rectangle`, nhưng nó sẽ trả lỗi nếu nhận vào hình vuông. Vấn đề cốt lõi ở đây là tác giả của *g* tin là `setWidth` và `setHeight` độc lập với nhau. 

Niềm tin này là có cơ sở vì việc thay đổi độ dài chiều dài của hình chữ nhật không nên thay đổi chiều rộng của nó! Tuy vậy, không phải tất cả các lớp con của hình chữ nhật đều có thể thỏa mãn niềm tin đó. Nếu ta đưa một `Square` vào một hàm như *g* mà tác giả có một niềm tin không được thỏa mãn, hàm *g* đó sẽ bị hỏng. Có thể nói hàm *g* dễ vỡ với sự có mặt của thiết kế `Square/Rectangle`.

Vì `Square` không thể được thay thể bởi một `Rectangle` trong hàm *g*, việc `Square` kế thừa từ `Rectangle` vi phạm LSP.

Dù vậy, vẫn có thể có ý kiến cho rằng chính hàm *g* mới là vấn đề ở đây vì tác giả của *g* không được phép đặt niềm tin rằng hai hàm của `Rectangle` độc lập với nhau, nhưng việc đó lại vô cùng hợp lý vì về mặt toán học, việc chiều dài và rộng của hình chữ nhật độc lập với nhau là hợp lý.
Nhưng đồng thời, tác giả của lớp `Square` cũng không vi phạm bất kỳ tính chất toán học nào.

### Sự hợp lý không phải tự nhiên
LSP dẫn chúng ta đến một kết luận rất quan trọng: một mô hình, khi được xem xét độc lập, không thể nào được kiểm chứng một cách thỏa đáng. Sự hợp lý của một mô hình chỉ có thể được thể hiện thông qua việc tương tác của nó với những lớp sử dụng nó. Như trong ví dụ trên, khi ta xem xét thiết kế của `Square` và `Rectangle` một cách độc lập, ta thấy chúng nhất quán và hợp lý. Tuy vậy, khi ta xem xét chúng dưới góc nhìn của một người có những niềm tin hợp lý về lớp ban đầu `Rectangle`, mô hình này là thể hiện những khiếm khuyết.

Vậy thì khi đánh giá một mô hình, ta không nên chỉ đánh giá nó một cách độc lập mà còn phải cân nhắc đến việc những lớp dùng các lớp này có thể có những niềm tin như thế nào về chúng.

Làm sao để  biết được người dùng những lớp này sẽ có những niềm tin gì vào thiết kế? Điều này là không dễ để dự đoán. Nếu chúng ta chuẩn bị cho tất cả mọi khả năng, thiết kế của ta có thể trở nên phức tạp không cần thiết (*Needlessly Complex*). Vì vậy, ta nên mặc kệ tất cả những vi phạm LSP ngoại trừ những thứ rõ ràng nhất cho đến khi tác hại của chúng trở nên rõ ràng.

### **Là một** là mối quan hệ về **Hành vi**
Quay lại ví dụ về hình vuông và hình chữ nhật, chuyện gì đã xảy ra? Chẳng `Square` lại không phải là `Rectangle`?

Đối với tác giả hàm *g*, một vật thuộc lớp `Square` chắc chắn không phải là `Rectangle`! Tại sao vậy? Bởi vì hành vi của lớp `Square` hoàn toàn không khớp với những gì mà tác giả hàm *g* mong đợi từ `Rectangle`. Về mặt hành vi, `Square` không phải là `Rectangle`, và trong phần mềm, hành vi là thứ ta quan tâm. LSP làm rõ ra rằng trong lập trình hướng đối tượng, mối quan hệ **là một** quan tâm đến những **hành vi** mà những lớp phụ thuộc có thể tin vào một cách hợp lý.

### Thiết kế với contract

Nhiều lập trình viên sẽ không cảm thấy thoải mái với việc phải xử lý việc hành vi phải hợp lý với những niềm tin của những lớp dùng nó. Làm sao biết được người dùng sẽ trông chờ điều gì? Có một kỹ thuật để làm rõ tất cả những điều hợp lý mà người dùng có thể trông đợi theo LSP. Kỹ thuật đó được gọi là thiết kế dựa trên hợp đồng (Design by Contract - DBC) và được đề xướng bởi Bertrand Meyer.

Sử dụng DBC, tác giả của một lớp có thể làm rõ những gì mà điều kiện cần để một hàm có thể được gọi, cũng như những gì mà kết quả của hàm sẽ đảm bảo sau khi hàm được gọi.

Ví dụ ta có thể xem xét đến điều kiện kết quả scó thể đảm bảo được của `Rectangle::setWidth`:

`assert((itsWidth == w) && (itsHeight == old.itsHeight))`

Trong ví dụ này, `old` là giá trị của `Rectangle` trước khi `setWidth` được gọi. Luật đối với điều kiện trước và sau của các lớp con được Meyer phát biểu như sau:

> Điều kiện yêu cầu của hàm ở lớp con phải bằng hoặc lỏng hơn so với điều kiện của hàm ở lớp cha. Kết quả đảm bảo của lớp con phải bằng hay nhiều hơn so với kết quả đảm bảo của lớp cha.