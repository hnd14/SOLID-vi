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

## L - Liskov Substitution Principle (LSP)

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

#### `Square` và `Rectangle`, một sự vi phạm khó nhận ra hơn

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

#### Vấn đề thực sự

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

#### Sự hợp lý không phải tự nhiên
LSP dẫn chúng ta đến một kết luận rất quan trọng: một mô hình, khi được xem xét độc lập, không thể nào được kiểm chứng một cách thỏa đáng. Sự hợp lý của một mô hình chỉ có thể được thể hiện thông qua việc tương tác của nó với những lớp sử dụng nó. Như trong ví dụ trên, khi ta xem xét thiết kế của `Square` và `Rectangle` một cách độc lập, ta thấy chúng nhất quán và hợp lý. Tuy vậy, khi ta xem xét chúng dưới góc nhìn của một người có những niềm tin hợp lý về lớp ban đầu `Rectangle`, mô hình này là thể hiện những khiếm khuyết.

Vậy thì khi đánh giá một mô hình, ta không nên chỉ đánh giá nó một cách độc lập mà còn phải cân nhắc đến việc những lớp dùng các lớp này có thể có những niềm tin như thế nào về chúng.

Làm sao để  biết được người dùng những lớp này sẽ có những niềm tin gì vào thiết kế? Điều này là không dễ để dự đoán. Nếu chúng ta chuẩn bị cho tất cả mọi khả năng, thiết kế của ta có thể trở nên phức tạp không cần thiết (*Needlessly Complex*). Vì vậy, ta nên mặc kệ tất cả những vi phạm LSP ngoại trừ những thứ rõ ràng nhất cho đến khi tác hại của chúng trở nên rõ ràng.

#### **Là một** là mối quan hệ về **Hành vi**
Quay lại ví dụ về hình vuông và hình chữ nhật, chuyện gì đã xảy ra? Chẳng `Square` lại không phải là `Rectangle`?

Đối với tác giả hàm *g*, một vật thuộc lớp `Square` chắc chắn không phải là `Rectangle`! Tại sao vậy? Bởi vì hành vi của lớp `Square` hoàn toàn không khớp với những gì mà tác giả hàm *g* mong đợi từ `Rectangle`. Về mặt hành vi, `Square` không phải là `Rectangle`, và trong phần mềm, hành vi là thứ ta quan tâm. LSP làm rõ ra rằng trong lập trình hướng đối tượng, mối quan hệ **là một** quan tâm đến những **hành vi** mà những lớp phụ thuộc có thể tin vào một cách hợp lý.

#### Thiết kế với contract

Nhiều lập trình viên sẽ không cảm thấy thoải mái với việc phải xử lý việc hành vi phải hợp lý với những niềm tin của những lớp dùng nó. Làm sao biết được người dùng sẽ trông chờ điều gì? Có một kỹ thuật để làm rõ tất cả những điều hợp lý mà người dùng có thể trông đợi theo LSP. Kỹ thuật đó được gọi là thiết kế dựa trên hợp đồng (Design by Contract - DBC) và được đề xướng bởi Bertrand Meyer.

Sử dụng DBC, tác giả của một lớp có thể làm rõ những gì mà điều kiện cần để một hàm có thể được gọi, cũng như những gì mà kết quả của hàm sẽ đảm bảo sau khi hàm được gọi.

Ví dụ ta có thể xem xét đến điều kiện kết quả scó thể đảm bảo được của `Rectangle::setWidth`:

`assert((itsWidth == w) && (itsHeight == old.itsHeight))`

Trong ví dụ này, `old` là giá trị của `Rectangle` trước khi `setWidth` được gọi. Luật đối với điều kiện trước và sau của các lớp con được Meyer phát biểu như sau:

> Điều kiện yêu cầu của hàm ở lớp con phải bằng hoặc lỏng hơn so với điều kiện của hàm ở lớp cha. Kết quả đảm bảo của lớp con phải bằng hay nhiều hơn so với kết quả đảm bảo của lớp cha.

Nói cách khác, khi dùng một vật dựa trên interface gốc, người dùng chỉ biết được các điều kiện yêu cầu cũng như kết quả đảm bảo của hàm ở lớp gốc. Vì vậy, các lớp kế thừa từ lớp gốc phải chấp nhận được các điều kiện mà lớp gốc có thể nhận được, cũng như đảm bảo được các kết quả do lớp gốc cam kết. 

Như vậy, trong ví dụ `Square` và `Rectangle`, kết quả đảm bảo của `Square::setWidth` không đủ so với `Rectangle::setWidth` vì nó không đảm bảo được điều kiện `(itsHeight == old.itsHeight)`.

Một số ngôn ngữ như *Eiffel* có hỗ trợ cho việc khai báo và kiểm tra các điều kiện đầu vào cũng như kết quả đảm bảo của các hàm ở rum-time. Cả *C++* và *Java* đều không có tính năng này.

#### Làm rõ cá điều kiện ở Unit Test

Unit test là một cách để lập trình viên ngầm khẳng định những điều kiện đầu vào cũng như kết quả của hàm do mình viết. Vì vậy, tác giả của các lớp sử dụng sẽ cần review UT do tác giả của lớp được dùng viết.

### Phân tích thay vì kế thừa

Một trường hợp thú vị nữa là mối quan hệ giữa `Line` (đường thẳng) và `LineSegment`(đoạn thẳng). Xét snippet *10-7* và *10-8*.

```cpp
geometry/line.h
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"

class Line 
{
    public:
        Line(const Point& p1, const Point& p2);

        double GetSlope() const;
        double GetIntercept() const;
        Point getP1() const {return itsP1;};
        Point getP2() const {return itsP2;};
        virtual bool isOn(const Point&) const;
    private:
        Point itsP1;
        Point itsP2;
}
#endif
```
*Snippet 10-7*
``` cpp
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"
class LineSegment: public Line 
{
    public:
        LineSegment(const Point& p1, const Point& p2);
        double GetLength() const;
        virtual bool isOn(const Point&) const;
}
#endif
```
*Snippet 10-8*

Người dùng lớp `Line` có thể trông đợi rằng tất cả những điểm thẳng hàng với `Line::itsP1` and `Line::itsP2` sẽ nằm trên `Line`. Tuy nhiên với `LineSegment`, điều này lại không đúng.

Vậy sao việc này có quan trọng không? Liệu ta có thể giữ nguyên thiết kế này và chấp nhận vấn đề tiềm ẩn này? Việc này là tùy thuộc vào quyết định của lập trình viên. Có những trường hợp việc chấp nhận sự sai số của thiết kế đem lại những lợi ích, và việc quyết định chúng có đáng để đánh đổi hay không là vấn đề kỹ thuật, và một kỹ sư tốt sẽ biết khi nào sự không hoàn hảo sẽ đem lại *lợi ích* lớn hơn. Tuy vậy, việc vi phạm LSP không nên bị đánh giá thấp. Việc đảm bảo được rằng các lớp con sẽ luôn hoạt động khi lớp cha được sử dụng là một cách rất tốt để kiểm soát độ phức tạp. Khi ta quyết định từ bỏ nó, ta sẽ cần phải xem xét từng lớp một cách riêng lẻ.

Trong trường hợp này, có một cách giải quyết đơn giản thể hiện được một công cụ quan trọng của lập trình hướng đối tượng là *phân tích (factoring)*. Nếu ta có thể toàn quyền thay đổi cả `Line` và `LineSegment`, ta có thể đơn giản là phân tích các điểm chung của chúng vào một lớp cha trừu tượng như ở snippet *10-9* đến *10-11*

```cpp
geometry/line.h
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"

class LinearObject
{
    public:
        LinearObject(const Point& p1, const Point& p2);

        double GetSlope() const;
        double GetIntercept() const;
        Point getP1() const {return itsP1;};
        Point getP2() const {return itsP2;};
        virtual int isOn(const Point&) const = 0; // abstract
    private:
        Point itsP1;
        Point itsP2;
}
#endif
```
*Snippet 10-9*
``` cpp
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"
class Line: public LinearObject 
{
    public:
        Line(const Point& p1, const Point& p2);
        virtual bool isOn(const Point&) const;
}
#endif
```
*Snippet 10-10*
``` cpp
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"
class LineSegment: public Line 
{
    public:
        LineSegment(const Point& p1, const Point& p2);
        double GetLength() const;
        virtual bool isOn(const Point&) const;
}
#endif
```
*Snippet 10-11*

`LinearObject` thể hiện cả `Line` và `LineSegment`. Nó chưa hầu hết các chức năng và dữ liệu của cả 2 lớp ngoại trừ phương thức `isOn` là trừu tượng hoàn toàn. Người dùng `LinearObject` hoàn toàn không thể đặt bất kỳ niềm tin nào về hàm `isOn`, và vì vậy, họ có thể nhận cả `Line` và `LineSegment` mà không gặp bất kỳ vấn đề gì. Hơn thế nữa, người dùng `Line` sẽ không bao giờ gặp `LineSegment`.

Phân tích như là một công cụ thiết kế nên được ứng dụng sớm, khi mà khối lượng code chưa nhiều. Rõ ràng là nếu đã có hàng tá lớp dùng `Line` ở *snippet 10-7*, việc phân tích ra `LinearObject` sẽ trở nên khó hơn rất nhiều. Tuy vậy, khi phân tích có thể được thực hiện, nó là một công cụ rất mạnh. Nếu các tính chất có thể được phân tích thành các lớp, có khả năng sẽ có những lớp khác xuất hiện sau sẽ có thể kế thừa các từ các lớp đó. Về việc này, Rebecca Wirfs-Brock, Brian Wilkerson, và Lauren Wiener phát biểu:
> Nếu ta có một tập hợp các lớp cùng hỗ trợ một số trách nhiệm giống nhau, chúng nên kế thừa các tính năng này từ cung một lớp cha.
> Nếu lớp cha này chưa tồn tại, hãy tạo và đưa những trách nhiệm này vào một lớp cha mới. Khả năng là phần mềm của bạn có thể sẽ được mở rộng để thêm những lớp mới cũng cần những chức năng này. Khi phân tích như vậy, lớp cha này có lẽ nên là một lớp trừu tượng.

*Snippet 10-12* cho thấy cách mà `LinearObject` được tái sử dụng bởi một lớp mà ban đầu ta không lường trước, `Ray` (tia). Một `Ray` có thể được thay thế cho một `LinearObject`, và không có lớp nào sử dụng `LinearObject` mà không thể xử lý `Ray`.

``` cpp
#ifndef GEOMETRY_LINE_H
#define GEOMETRY_LINE_H
#include "geometry/point.h"
class Ray: public Line 
{
    public:
        Ray(const Point& p1, const Point& p2);
        virtual bool isOn(const Point&) const;
}
#endif
```
*Snippet 10-12*

### Những heuristic và ứng dụng thường gặp

Có một vài heuristic đơn giản để nhận ra những vi phạm LSP, và nó thường liên quan đến việc lớp con *bỏ bớt* các chức năng từ lớp cha. Nếu một lớp con làm ít hơn lớp cha, nó thường không thể được thay thế bằng lớp cha và từ đó dẫn đến vi phạm LSP.

#### Chức năng bị thoái hóa ở lớp con

Xét *snippet 10-13*. Hàm *f* ở `Base` được triển khai, nhưng ở `Derived` thì lại bị thoái hóa, có lẽ vì tác giả của `Derived` thấy rằng nó không có ứng dụng gì ở đây. Tuy vậy, không may là các người dùng của `Base` lại không biết gì về việc không nên gọi *f* đối với `Derived`, dẫn đến việc vi phạm thay thế.
```cpp
public class Base
{
    public void f() {/*some code*/}
}
public class Derived extends Base
{
    public void f() {}
}
```
*Snippet 10-13*

Việc xuất hiện các hàm thoái hóa không phải lúc nào cũng có nghĩa là vi phạm LSP, nhưng ta vẫn nên chú ý đến chúng khi chúng xuất hiện.

#### Ném lỗi ở lớp con

Một cách vi phạm LSP thường gặp khác là việc lỗi được ném ở các lớp con. Nếu người dùng lớp cha không chuẩn bị trước việc hàm có thể ném lỗi thì việc ném lỗi ở lớp con làm nó không thể thay thế cho lớp cha. Để giải quyết việc này, hoặc là lớp con phải không quăng các lỗi không lường trước, hoặc là lớp cha phải thông báo việc quăng lỗi có thể xảy ra.

### Kết luận

Nguyên lý đóng mở (OCP) là trái tim của rất nhiều ưu điểm của lập trình hướng đối tương. Khi nguyên lý này được đảm bảo, phần mêm trở nên dễ bảo quản và tái sử dụng hơn. LSP là điều kiện chính yếu cho phép OCP được đảm bảo. Việc subtype có thể thay thế được cho base type cho phép các modules được định nghĩa với base type được mở rộng mà không cần phải thay đổi. Việc có thể thay đổi được base type bằng subtype phải là một điều mà các lập trình viên có thể dựa vào. Vì vậy, các điều kiện yêu cầu và đảm bảo của lớp base phải được làm rõ và hiểu rõ, hay bắt buộc tường minh, bằng code.

Cụm từ "là một" là hơi quá rộng để diễn tả tính kế thừa. "Có thể thay thế được" là một mô tả chính xác hơn của tính kế thừa, mà trong đó việc "Có thể thay thế được" phải được định nghĩa rõ ràng bằng các ràng buộc ngầm định hay tường minh.

## D - Dependency Inversion Principle (DIP)
*Nguyên lý đảo ngược phụ thuộc*

### Phát biểu DIP
> a. Các high-level modules không nên dựa vào cá c low-level modules. Cả hai nên dựa vào trừu tượng hóa.

> b. Các lớp trừu tượng không nên dựa vào các lớp chi tiết. Các lớp tường minh (chi tiết) nên dựa vào các lớp trừu tượng.

Qua thời gian, nhiều người từng đặt câu hỏi tại sao lại dùng đảo ngược (Inversion) trong tên nguyên lý này. Đó là vì những phương pháp thiết kế truyền thống như Phân tích và thiết kế theo cấu trúc (Structured analysis and design - SA/SD) thường tạo ra các kiến trúc phần mềm mà các module ở tầng cao (high-level) sẽ phụ thuộc vào các module ở tầng thấp hơn (low-level), mà cụ thể hơn là các chính sách (*policy*) sẽ phụ thuộc vào các chi tiết (*detail*). Điều này là tất yếu khi mà một trong những mục tiêu của phương pháp này là định nghĩa việc các module ở tầng cao sẽ gọi các hàm ở module tầng thấp như thế nào. Thiết kế của chương trình `Copy` ở *hình 7-1* là một ví dụ rõ ràng cho việc này. Việc phụ thuộc trong một chương trình hướng đối tượng được thiết kế tốt sẽ "ngược" với thiết kế có được từ những phương pháp thiết kế truyền thống.

Xem xét hậu quả của việc các module ở tầng cao phụ thuộc vào các module ở tầng thấp. Những module này sẽ chứa những chính sách quan trọng quyết định mục đích của chương trình. Tuy vậy, việc những module này phụ thuộc vào những module cấp thấp hơn dẫn đến khi những module cấp thấp bị thay đổi sẽ ảnh hưởng đến những module này.

Việc này là vô cùng bất hợp lý! Những module tầng cao là thứ quyết định tính chất của chương trình lẽ ra phải là những module có khả năng gây ảnh hưởng lên các module cấp thấp. Những module tầng cao chứa những chính sách và logic mật thiết đến kết quả của chương trình phải được ưu tiên hơn, và nên độc lập khỏi những module tầng thấp chứa những lập trinh chi tiết. Các module tầng cao không nên phụ thuộc vào các module tầng thấp theo bất kỳ cách nào. 

Hơn thế nữa, chính các module tầng cao mới chính là những thứ mà ta muốn tái sử dụng. Việc tái sử dụng các module ở cấp thấp vốn đã khá dễ dàng thông qua các thư viện subroutines. Khi mà các module ở level cao phụ thuộc vào các module cấp thấp, việc tái sử dụng các module cấp cao cho các ngữ cảnh khác nhau trở nên vô cùng khó khăn. Ngược lại, khi các module cấp cao không phụ thuộc vào các module cấp thấp, việc tái sử dụng chúng sẽ trở nên cực kỳ dễ dàng. Đây chính là vấn đề cốt lõi của việc thiết kê framework.

### Layering

Theo Brooch, tất cả các thiết kế hướng đối tượng tốt đều gồm những tầng lớp tách biệt được định nghĩa rõ ràng mà mỗi tầng lớp sẽ cung cấp một bộ các dịch vụ nhất quán thông qua một bộ interface rõ ràng và liền mạch. Việc hiểu phát biểu này một cách ngây thơ sẽ dẫn đến một thiết kế tương tự như trong *Hình 11-1*. Trong hình này, tâng `Policies` ở cao nhất sẽ sử dụng tầng `Mechanism` ở dưới, và tầng `Mechanism` sẽ sử dụng tầng `Utility` ở thấp nhất. Mặc dù thiết kế này nhìn có vẻ hợp lý, nó lại có một nhược điểm chí mạng là tầng `Policies` sẽ nhạy cảm với các thay đổi ở tất cả các tầng thấp hơn cho tới tận tầng `Utility`. Điều này là bởi sự phụ thuộc có tính bắc cầu. Vì tầng `Policies` phụ thuộc vào một thứ phụ thuộc vào tầng `Utilities` nên `Policies` sẽ gián tiếp phụ thuộc vào `Utilities`, và những thay đổi ở `Utilities` có thể sẽ gây ảnh hưởng lên `Policy`. Điều này quả thực là đáng tiếc.

*Hình 11-2* là một thiết kế cải tiến hơn. Mỗi tầng sẽ khai báo một bộ các interface trừu tượng để diễn tả những service mà nó cần dùng. Sau đó, tầng thấp hơn sẽ hiện thực hóa các interface trừu tượng ở tầng ngay trên của nó. bằng cách này, các lớp thuộc tầng trên sẽ không phụ thuộc vào các lớp ở tầng dưới mà thay vào đó sẽ phụ thuộc và các lớp trừu tưởng ở cùng tầng. Như vậy, không chỉ sự phụ thuộc của `PolicyLayer` vào `UtilityLayer` bị phá vỡ, mà cả sự phụ thuộc của `PolicyLayer` vào `MechanismLayer` cũng bị bẻ gãy.

Điều này đôi khi còn được biết đến với tên gọi là nguyên lý Hollywood: "Đừng gọi chúng tôi, chúng tôi sẽ gọi bạn". Khi đó, những module ở tầng thấp sẽ cung cấp những triển khai chi tiết cho các interface được khai báo và gọi bởi các module ở tầng cao hơn.

Bằng cách thay đảo ngược sự phụ thuộc như thế này `PolicyLayer` sẽ trở nên bền vững trước những thay đổi ở `MechanismLayer` cũng như `UtilityLayer`. Hơn thế nữa, `PolicyLayer` có thể được tái sử dụng cho bất cứ modules thấp hơn nào tuân thủ `PolicyServiceInterface`. Vậy là bằng cách đảo ngược sự phụ thuộc, ta thu lại được một cấu trúc vừa mêm dẻo, bền bỉ, và dễ tái sử dụng hơn.

#### Phụ thuộc vào trừu tượng

Một cách hiểu hơi ngây ngô nhưng vẫn rất hiệu quả về DIP được diễn tả bởi heuristic đơn giản sau: Luôn phụ thuộc vào sự trừu tượng. Hiểu đơn giản, heuristic này khuyên ta rằng ta nên tránh phụ thuộc và các lớp cụ thể, hay nói khác đi, tất cả các mối quan hệ trong một phần mềm đều phải được kết thúc ở một abstract class hay interface.

Theo Heuristic này:
- Không biến nào nên giữ một con trỏ đến một lớp cụ thể
- Không lớp nào nên kế thừa từ các lớp cụ thể
- Không method nào nên override một method đã được định nghĩa

Tất nhiên, heuristic này chắc chắn sẽ bị vi phạm ít nhất một lần ở trong tất cả các chương trình. Ai đó sẽ phải khởi tạo những instances của các lớp cụ thể, và module nào làm việc đó chắc chắn sẽ phụ thuộc vào các lớp cụ thể này. Hơn thế nữa, chẳng có lý do gì khiến ta phải tuân theo heuristic này cho các lớp cụ thể nhưng không thay đổi nhiều. Nếu một lớp cụ thể không phải thay đổi gì nhiều, và cũng chẳng có lớp nào được mở rộng từ nó, việc phụ thuộc vào nó sẽ chẳng có hại gì cả.

Một ví dụ đơn giản, trong hầu hết mọi hệ thống, lớp biểu diễn các chuỗi thường là lớp cụ thể. Ví dụ như trong *Java* thì `String` là một lớp cụ thể. Tuy vậy, lớp này thì không thường thay đổi, nên việc phụ thuộc vào nó chẳng có gì hại cả.

Tuy nhiên, cũng cần lưu ý rằng các lớp cụ thể do ta định nghĩa thì rất tiếc là lại rất thường xuyên bị thay đổi. Chính những lớp cụ thể đó là thứ mà ta không muốn dựa vào một cách trực tiếp. Những thay đổi phải thực hiện ở những lớp này nên được cô lập đằng sau các interface trừu tượng.

Nhưng điều này cũng chưa hoàn hảo. Đôi khi những thay đổi của một lớp sẽ gây ảnh hưởng đến interface hướng ra ngoài của lớp này. Khi đó, những thay đổi này sẽ cần phải được chuyển đến những interface trừu tượng, phá vỡ sự cô lập được tạo ra bởi các lớp trừu tượng.

Đây chính là lý do dẫn đến việc heuristic này hơi ngây thơ. Mặt khác, nếu ta hiểu theo một cách đúng đắn hơn rằng mỗi client sẽ tự định nghĩa các hàm interface mà chúng cần gọi, vậy thì các interface này sẽ chỉ cần thay đổi khi mà các *client* cần những thay đổi đó. Những thay đổi ở các lớp cụ thể hóa các interface sẽ không gây ảnh hưởng gì đến các *client*.

### Một ví dụ đơn giản

DIP có thể được áp dụng bất cứ khi nào một lớp cần gửi một tín hiệu đến một lớp khác. Xét ví dụ dưới đây về `Button` và `Lamp`. 

Đối tượng `Button` sẽ cảm nhận môi trường xung quanh, và khi nhận được tín hiệu gọi `poll`, nó sẽ xác định việc có một người dùng nào đó đã *bấm* nút. Việc bấm như thế nào là không quan trọng. Nó có thể là một cái nút vật lý, cũng có thể là một cái nút ảo trên một *GUI*, hay cũng có thể là một cảm biến chuyển động trong một hệ thống an ninh tại gia. `Button` chỉ xác định là đã có người dùng bật hay tắt nó.

Đối tượng `Lamp` lại tạo ra ảnh hưởng lên môi trường xung quanh. Khi nhận được lệnh `TurnOn`, nó sẽ tỏa sáng lên môi trường xung quanh, và khi nhận lệnh `TurnOff`, nó sẽ tắt ánh sáng này đi. Cơ chế vật lý cho chuyện tỏa sáng này cũng không quan trọng. Đây có thể là một đèn LED, đèn huỳnh quang hay thậm chí là đèn laser trong một máy in laser.

Vậy làm sao để ta thiết kế một hệ thống mà trong đó `Button` sẽ điều khiển `Lamp`? *Hình 11-3* là một thiết kế ngây thơ mà trong đó sau khi `Button` nhận được lệnh `poll`, nó sẽ xác định đã được bấm, và sẽ gửi các lệnh `TurnOn` hoặc `TurnOff` đến `Lamp`.

Tại sao thiết kế này lại ngây thơ? Xem xét đến *snippet 11-1* triển khai thiết kế này với *Java*. Việc `Button` phụ thuộc vào `Lamp` nghĩa là `Button` sẽ bị ảnh hưởng bỏi các thay đổi của `Lamp`. Hơn thế nữa, việc tái sử dụng `Button` để điều khiển `Motor` là không thể. Trong thiết kế này, các `Button` điều khiển các `Lamp` và chỉ các `Lamp`.

**Button.java**
```Java
public class Button 
{
    private Lamp itsLamp;
    public void poll() 
    {
        if (/*some conditions*/)
            itsLamp.turnOn()
    }

}
```
Thiết kế này vi phạm DIP, các chính sách ở tầng cao vẫn chưa được tách bạch khỏi các triển khai ở tầng thấp. Những khái niệm trừu tượng vẫn chưa được tách ra khỏi những triển khai cụ thể. Nếu không có sự tách biệt này, những modules ở tầng cao sẽ nghiễm nhiên phải phụ thuộc vào các triển khai cụ thể ở dưới thấp, và sự trừu tượng chắc chắn phải phụ thuộc lên những triển khai cụ thể.
## I - Interface Segration Principles

Nguyên lý này xử lý vấn đề của các "fat" interface. Các lớp có "fat" interface là các lớp mà các hàm chỉ ra không nhất quán. Nói cách khác, các hàm show ra ngoài của lớp có thể được tách ra làm các nhóm hàm khác nhau phục vụ cho các đối tượng khách hàng khác nhau. Vì vậy, một số đối tượng khách hàng dùng một số chức năng trong khi các đối tượng khách hàng khác dùng các nhóm chức năng khác.

ISP thừa nhận rằng đôi khi rằng đôi khi các vật thể yêu cầu phải có một bộ các interface không nhất quán. Tuy vậy, các khách hàng không nên biết đến các lớp như vậy dưới dạng một class hoàn chỉnh mà chỉ nên biết về các lớp đó thông qua các lớp trừu tượng gốc mà interface của chúng là nhất quán.

### Ô nhiễm interface
Xét đến một hệ thống bảo mật mà trong đó các vật thể `Door` có thể được khóa hoặc hoặc không khóa, và có thể biết được trạng thái của nó là đóng hay mở. 

```cpp
Security Door
class Door
{
   public:
      virtual void Lock() = 0;
      virtual void Unlock() = 0;
      virtual bool IsDoorOpen() = 0;
};
```

Lớp này là trừu tượng để các khách hàng có thể dùng tất cả các objects thỏa mãn interface này mà không phụ thuộc vào bất kỳ implementation cụ thể nào của `Door`.

Bây giờ xét một trường hợp là implementation cụ thể `TimedDoor` cần phải tạo một thông báo nêú cửa được mở quá lâu. Để làm được việc này, `TimedDoor` cần phải tương tác với một vật phẩm khác là `Timer` (*snippet 12-2*).

```cpp
class Timer
{
    public:
        void Register(int timeout, TimerClient* client);
};
class TimerClient
{
    public:
        virtual void TimeOut() = 0;
};
```

Khi một vật muốn được thông báo về việc time-out, nó sẽ gọi đến hàm `Register` của timer để đăng ký. Trong đó argument `timeout` của hàm `Register` là chỉ thời gian cảu việc time-out, và nó sẽ cung cấp thêm một con trỏ đến một vật `TimerClient` mà khi đến lúc nó sẽ gọi hàm `Timeout` cho vật đó.

Vậy làm thế nào ta có thể làm cho `TimerClient` liên hệ đến `TimedDoor` để code ở trong `TimedDoor` có thể được thông báo khi đến lúc time-out? Có một vài cách khác nhau. Hình *12-1* là một cách làm ngây thơ. Ta bắt `Door`, và từ đó dẫn đến `TimedDoor`, kế thừa từ `TimerClient`. Điều này đảm bảo rằng `TimerClient` có thể được đăng ký với `Timer` và nhận được tin nhắn `TimeOut`.

Mặc dù đây là cách làm khá thường gặp, có những vấn đề với cách này. Nổi cộm nhất ở đây là việc lớp `Door` bây giờ phụ thuộc vào `TimerClient`. Không phải loại cửa nào cũng sẽ cần `TimerClient`. Thực tế, `Door` trừu tượng ban đầu chẳng có gì dính dáng đến `TimerClient`. nếu những loại door không cần thời gian kế thừa từ `Door`, chúng sẽ phải tạo một implementation tiêu biến cho `TimeOut` dẫn đến nguy cơ vi phạm LSP. Hơn thế nữa, những ứng dụng cần dùng các lớp `Door` mà không cần `Timer` bây giờ vẫn sẽ phải import định nghĩa của `TimerClient` mặc dù chúng chẳng cần dùng đến. Điều này là dấu hiệu của *Dư thừa* (Needless redundancy) và *Phức tạp không cần thiết* (Needless Complexity).

Đây là một ví dụ của ô nhiễm interface, một bệnh thường gặp trong những ngôn ngữ lập trình *loại cố định* (statically typed) như *C++* và *Java*. Interface của `Door` đã bị ô nhiễm bợi một hàm mà nó không cần đến. Nó bị bắt phải mang theo hàm này bên trong vì lợi ích của một trong các lớp con của nó. Nếu việc này bị tiếp tục, mỗi khi một lớp con của nó cần một hàm nào đó, hàm đó sẽ được thêm thẳng vào lớp gốc và làm interface của lớp gốc trở nên ô nhiễm hơn và trở thành "fat". 

Hơn thê nữa, mỗi khi một hàm đươc thêm vào base class, hàm đó phải được triển khai (hoặc là được áp dụng mặc định) ở trong các lớp con. Và thực tế là khi những hàm này được thêm vào base class, một tiêu biến mặc định sẽ được thêm vào lớp gốc để cho các lớp khác tránh phải gánh trách nhiệm triển khai các hàm này. Như ta đã biết từ trước, việc này là vi phạm LSP và có thể dẫn đến các khó khăn cho việc maintenance cũng như tái sử dụng.

### Các interface riêng biệt cho các khách hàng riêng biệt

`Door` và `TimerClient` là các interface phục vụ cho các đối tượng khách hàng khác biệt hoàn toàn. `TimerClient` là để phục vụ cho `Timer`, còn `Door` là để phục vụ các lớp cần tương tác với các thể loại cửa. Vì các đối tượng khách hàng là khác biệt, các interface cũng cần được rạch ròi. Vì sao? Bởi vì các khách hàng cũng có thể tác động lên các interface mà chúng dùng.

#### Lực tác động ngược của khách hàng lên các interface

Khi ta nghĩ đến các nguyên nhân dẫn đến phần mềm cần được thay đổi, ta thường nghĩ đến việc các thay đổi ở interface sẽ ảnh hưởng đến những khách hàng sử dụng interface đó như thế nào. Ví dụ như ta thường chỉ nghĩ đến rằng việc thay đổi `TimerClient` sẽ ảnh hưởng như thế nào đến các lớp dùng `TimerClient`. Tuy vậy, các lớp dùng `TimerClient` cũng có khả năng tác động lên `TimerClient và buộc nó phải thay đổi.

Ví dụ, một vài đối tượng của dùng timer sẽ đăng ký nhiều hơn 1 lần timeout. Xét trường hợp của `TimedDoor`. Khi nhận ra rằng cửa được mở, nó sẽ gửi một tín hiệu `Register` đến `Timer` để đăng ký một lần timeout. Tuy vậy, trước khi thời gian timeout kết thúc, nếu cửa được đóng lại và mở ra thêm một lần nữa, nó sẽ lại gửi thêm một tín hiệu đến `Timer` để đăng ký một lần timeout mới. Sau đó, tín hiệu timeout đầu tiên hết thời gian và sẽ kích hoạt `TimeOut` của `TimedDoor`, tạo ra một báo động giả.

Ta có thể khắc phục vấn đề này bằng cách chỉnh sửa như *snippet 12-3*. Bằng việc thêm một *timeOutId* code độc nhất cho mỗi lần đăng ký, `TimẻClient` sẽ biết được rằng tín hiệu timeout đã được đăng ký nào đang trigger `TimeOut`.

```cpp
Timer with ID
class Timer
{
    public:
        void Register(int timeout,
                      int timeOutId,
                      TimerClient* client);
};
class TimerClient
{
    public:
        virtual void TimeOut(int timeOutId) = 0;
};
```

Rõ ràng thay đổi này sẽ gây ảnh hưởng tới tất cả các lớp phụ thuộc vào *TimerClient*. Ta chấp nhận việc này vì việc không có `timeOutId` là một thiếu sót trong quá trình thiết kế `Timer` cần phải được sửa đổi. Tuy vậy, thiết kế ở *snippet 12-1* buộc `Door` và tất cả các lớp phụ thuộc vào `Door` bị vạ lây. Đây là một dấu hiệu rõ ràng của tính nhớt và tính cứng nhắc. Tại sao một bug ở `TimerClient` lại gây ảnh hưởng lên những lớp phụ thuộc vào các lớp con không cần hẹn giờ của `Door`. Khi sự thay đổi ở một phần của phần mềm gây ảnh hưởng lên một phần khác chẳng hề liên quan trong chương trình, hậu quả của những thay đổi này trở nên khó đoán, và nguy cơ vỡ vụn trước thay đổi của chương trình trở nên rất cao.

### ISP: Nguyên lý rạch ròi interface

> Các khách hàng không nên bị ép phải phụ thuộc vào những hàm mà họ không dùng đến.

Khi mà các lớp khách hàng phải phụ thuộc lên những hàm họ không dùng đến họ sẽ bị ảnh hưởng bởi các thay đổi tiềm tàng của các hàm này. Điều này dẫn đến sự ràng buộc không đoán trước được giữa các lớp phụ thuộc. Nói khác đi, khi một client phụ thuộc vào một hàm nó không dùng đến nhưng được dùng bởi một lớp khác, nó sẽ chịu ảnh hưởng bởi những thay đổi lên hàm đó do những lớp dùng hàm đó gây ra. Ta muốn tránh những sự ràng buộc này bất kỳ lúc nào có thể, vì vậy nên ta cần sự rạch ròi giữa các interface.

### Interface của lớp v. interface của đối tượng



