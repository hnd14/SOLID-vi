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
