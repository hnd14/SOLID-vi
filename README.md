# Thiết kế Agile - Nguyên Lý SOLID
*AGILE SD PRINCIPLES, PATTERNS, and PRACTICES by Robert C. Martin*
## Thế nào là thiết kế agile
In 1992. Jack Reeves viết một bài luận trên tạp chí *C++ Journal* tựa là *"Thế nào là thiết kế phần mềm?"* Trong bài báo này, Reeves lập luận rằng thiết kế của một phần mềm được document chủ yếu dựa trên source code của nó. Những bản vẽ thiết kế chỉ là phụ trợ để thể hiện thiết kế chứ không phải là bản thân thiết kế. Bài báo của Jack chính là ngòi nổ cho việc phát triển phần mềm Agile.

Trong những trang sau đây, ta thường sẽ nói đến các "bản thiết kế" của các phần mềm. Đừng hiểu lầm điều này nghĩa là một tập hợp các bản vẽ UML tách biệt khỏi code. Tập hợp các bản vẽ UML có thể diễn tả một phần của bản thiết kế, nhưng nó không phải là bản thiết kế. Bản thiết kế của một phần mềm là một khái niệm trừu tượng liên quan trực tiếp đến hình hài và cấu trúc của phần mềm cũng như từng module, lớp, và hàm. Nó có thể được biểu diễn dưới nhiều phương diện khác nhau, nhưng cấu thành cuối cùng của nó là nằm ở trong code. Cuối cùng nhất, source code chính là "Bản thiết kế".

### Những tai nạn nào có thể xảy ra với một phần mềm?

Nếu bạn may mắn, bạn sẽ bắt đầu dự án với một bức tranh rõ ràng về những gì bạn mong muốn ở hệ thống bạn đang xây dựng. Bản thiết kế lúc này là một hình ảnh quan trọng trong đầu bạn. Nếu may mắn hơn nữa, bản thiết kế trong đầu bạn sẽ được hiện thực hóa rõ ràng ở bản release đầu tiên.

Sau đó, mọi thứ bắt đầu chệch khỏi quỹ đạo. Phần mềm bắt đầu trở nên bốc mùi như một miếng thịt thiu. Theo thời gian, mùi hôi này bắt đầu lan rộng ra, những điểm xấu bắt đầu tích tụ dần trong phần mềm, việc bảo trì ngày càng trở nên khó hơn. Cuối cùng, những thay đổi dù chỉ nhỏ nhất cũng yêu cầu một lượng công sức khổng lồ đến nổi tất cả các lập trình viên và quản lý phải xem xét đến việc tái cấu trúc lại hoàn toàn hệ thống.

Nhưng mà việc này thường rất ít khi thành công. Những nhà thiết kế bắt đầu với mục tiêu tốt, nhưng họ lại sớm nhận ra mình đang phải đuổi theo một mục tiêu di động. Hệ thống cũ tiếp tục phát triển và thay đổi, và thiết kế mới sẽ cứ phải chạy đuổi theo những thay đổi đó. Những yêu cầu và điểm nhức nhối cứ liên tục được thêm vào thiết kế mới trước cả khi nó được đến với release đầu tiên.

### Mùi thiết kế - dấu hiệu nhận biết phần mềm xuống cấp

Bạn có thể cảm nhận được sự xuống cấp của phần mềm khi chúng bắt đầu thể hiện các dấu hiệu dưới đây:
- **Sự cứng nhắc (Rigidity)** - hệ thống trở nên khó thay đổi vì một thay đổi ở một phần sẽ kéo theo hàng loạt thay đổi ở những phần khác.
- **Sự dễ vỡ (Fragility)** - Những thay đổi lên hệ thống khiến nó vỡ ở những phần mà lẽ ra chẳng có liên hệ gì với phần bị thay đổi
- **Sự ục ịch (Immobility)** - Rất khó để tách hệ thống ra thành các phần riêng biệt có thể tái sử dụng ở những hệ thống khác
- **Sự nhớt (Vicosity)** - Làm đúng cách trở nên khó hơn nhiều so với việc làm sai cách.
- **Phức tạp thừa thãi (Needless Complexity)** - Có những phần trong thiết kế chẳng nhằm mục đích gì
- **Lặp lại thừa thãi (Needless Repetition)** - Có những phần trong thiết kế bị lặp lại mà ta có thể thống nhất thông qua 1 lớp trừu tượng
- **Khó đọc hiểu(Opacity)** - Source code rất khó đọc hiểu, ý định của nó không được diễn tả rõ ràng.

**Sự cứng nhắc** Một phần mềm cứng nhắc là một phần mềm khó thay đổi. Một thiết kế cứng nhắc khi một thay đổi nhỏ trong nó kéo theo một loạt thay đổi ở các module phụ thuộc. Càng nhiều module phải thay đổi, thiết kế đó càng cứng nhắc 

Hầu hết các lập trình viên đều đã gặp những trường hợp tương tự như thế này. Họ được yêu cầu một thay đổi tưởng chừng là đơn giản. Họ nhìn qua và ước lượng một lượng công sức thỏa đáng cần để thực hiện thay đổi này. Sau đó, khi họ bắt tay vào thực hiện nó, họ bắt đầu nhận ra rằng những thay đổi này có những hệ quả khôn lường, và họ lại phải đi xử lý những hệ quả này như thể đang chơi mèo đuổi chuột, chỉnh sửa một lượng module lớn hơn rất nhiều so với những ước lượng ban đầu. Cuối cùng, việc thay đổi tốn thời gian và công sức nhiều hơn hẳn những ước lượng ban đầu, và khi được hỏi tại sao ước lượng ban đầu lại khác với thực tế nhiều đến vậy, họ chỉ biết nhún vai và lặp lại lý do quen thuộc của lập trình viên: "Nó phức tạp hơn tôi nghĩ nhiều".

**Sự dễ vỡ** - Phần mềm dễ vỡ là một phần mềm gặp lỗi ở nhiều nơi khi chỉ một thay đổi nhỏ xảy ra. Thông thường, những lỗi này xảy ở những nơi mà lẽ ra chẳng có liên hệ gì đến nơi được thay đổi, và việc sửa những lỗi này lại dẫn đến những lỗi mới, và đội ngũ lập trình bắt đầu trông như một chú chó đuổi theo cái đuôi của chính mình.

Khi sự dễ vỡ của một module tăng lên, xác suất lỗi xảy ra khi có thay đổi càng trở nên chắc chắn. Điều này nghe cực kỳ vô lý, nhưng những module như thế lại chẳng hề hiếm gặp. Đây là những module luôn cần được sửa - thường trực trong danh sách bug, ai cũng biết cần được thiết kế lại (nhưng chẳng ai muốn phải thiết kế lại), và càng sửa thì lại càng lỗi.

**Sự ục ịch** Một thiết kế ục ịch là một thiết kế chứa những phần hữu ích có thể được tái sử dụng ở những hệ thống khác, nhưng công sức cũng như rủi ro cho việc tách những phần đó ra khỏi hệ thống ban đầu là quá lớn. Đây là một điều rất đáng tiếc nhưng lại cũng rất thường gặp

**Sự nhớt** Có 2 loại nhớt: Phần mềm nhớt và môi trường nhớt.

Khi cần thực hiện một thay đổi, lập trình viên có nhiều cách để đạt được mục đích. Có những cách thì vẫn giữ nguyên tầm nhìn của thiết kế ban đầu, lại cũng có những thay đổi không làm được điều đó (những thay đổi như thế thì được gọi là hacks). Khi những thay đổi giữ nguyên thiết kế của phần mềm khó được thực hiện hơn các hack, ta nói rằng phần mềm có tính nhớt cao. Ta muốn thiết kế phần mềm sao cho việc thực hiện những thay đổi đúng đắn bảo toàn triết lý của phần mêm ban đầu dễ thực hiện hơn so với việc sử dụng các hack.

Nhớt ở môi trường xảy ra khi mà môi trường làm việc chậm chạp và không hiệu quả. Ví dụ như nếu việc compile code rất tốn thời gian, các lập trình viên sẽ bị cám dỗ để thực hiện các thay đổi sao cho phải compile lại ít file nhất có thể, mặc cho việc đó có bảo toàn thiết kế hay không. Hay là khi môi trường quản lý source code cần vài tiếng đồng hồ chỉ để check in một vài file, các lập trình viên lúc này sẽ bị cám dỗ đến việc làm sao để cần phải check-in càng ít file càng tốt mặc kệ design bị ảnh hưởng thế nào.

Trong cả 2 trường hợp, một dự án nhớt là một dự án mà triết lý của thiết kế rất khó để được bảo toàn. Chúng ta muốn tạo ra một phần mềm cũng như môi trường dự án sao cho việc bảo toàn triết lý thiết kế là dễ nhất có thể.

**Phức tạp thừa thãi** - Một thiết kế phức tạp thừa thải là khi mà nó chứa những chi tiết không có ứng dụng gì ở thời điểm hiện tại. Điều này xảy ra thường xuyên khi các lập trình viên dự đoán trước những thay đổi có thể xảy ra, và chuẩn bị trước cho những thay đổi này. Thoạt nhìn, điều này trông như là một điều nên làm. Việc chuẩn bị sẵn sàng trước cho các thay đổi trong tương lai sẽ làm code của ta linh hoạt hơn và tránh những thay đổi ác mộng trong tương lai.

Đáng tiếc thay, kết quả của việc này lại hoàn toàn ngược lại. Với việc chuẩn bị cho quá nhiều thay đổi, thiết kế trờ nên trì trệ do phải gánh những phần mà chẳng bao giờ dùng đến. Một số sự chuẩn bị có thể được đền đáp, nhưng rất nhiều những sự chuẩn bị khác thì không may mắn như vậy. Trong khi đó, thiết kế của bạn phải mang theo vô số những thành phần không hề cần thiết khiến nó trở nên phức tạp và khó hiểu.

**Lặp lại thừa thải** - Cut và paste là những công cụ vô cùng hữu hiệu khi bạn cần chỉnh sửa văn bản, nhưng nó lại chứa những hiểm họa khổng lồ khi dùng như một công cụ chỉnh sửa code. Việc phần mềm được tạo ra từ hàng trăm mảnh code bị lặp lại là quá thường gặp. Nó diễn ra như sau:

Ralph cần một đoạn code để làm một việc gì đó. Anh ấy nhìn quanh những phần khác của source code mà anh ấy nghĩ rằng là việc anh ấy cần làm phải được thực hiện và tìm ra một đoạn code phù hợp. Anh ấy liền cắt và dán đoạn code đó vào module của mình, rồi thực hiện những thay đổi cần thiết.

Nhưng Ralph không hề biết rằng đoạn code mà anh ấy đã chép ấy được viết bởi Todd, người cũng đã copy đoạn code thực hiện việc tương tự từ Lily. Lily là người đầu tiên làm việc mà Ralph muôn làm, nhưng cô ấy lại nhận thấy việc này cũng rất giống với một hành động khác, cô ấy cũng copy đoạn code thực hiện hành động đó về và chỉnh sửa cho phù hợp để làm công việc này.

Khi những đoạn code tương tự cứ xuất hiện lặp đi lặp lại, ở đâu đó, các lập trình viên đã thiếu một sự trừu tượng hóa. Tìm tất cả những nơi mà code bị lặp và refactor lại nó với một sự trừu tượng hóa phù hợp có lẽ không ở cao trong thứ tự ưu tiên công việc, nhưng việc đó sẽ khiến việc đọc hiểu và bảo trì hệ thống trở nên dễ dàng hơn rất nhiều.

Khi code bị lặp lại không cần thiết ỏ nhiều nơi, việc thay đổi hệ thống trờ nên khó nhằn. Bugs được tìm thấy ở trong một phần bị lặp lại như vậy sẽ phải được chỉnh sửa ở tất cả những nơi bị lặp lại, nhưng vì ở mỗi nơi lặp lại, code lại được chỉnh sửa đi một ít, việc sửa bug ở mỗi nơi đôi khi cũng phải khác đi.

**Khó đọc hiểu** - Code có thể được viết theo cách rõ ràng dễ hiểu hoặc rối rắm khó hiểu. Code được phát triển theo thời gian thường càng ngày càng trở nên rối rắm hơn. Vì vậy, đội ngũ lập trình cần phải để tâm đến việc giữ cho code rõ ràng và dễ hiểu.

Khi các lập trình viên vừa viết một module, module này trông có vẻ rõ ràng với họ. Đó là vì họ đã đắm chìm vào code trong khi phát triển, và họ hiểu được nó ở một mức độ cao hơn. Về sau, khi họ dần quên mất những hiểu biết về đoạn code, khi họ xem lại module cũ, họ lại chỉ biết tự hỏi là sao mình có thể viết được đoạn code tệ đến vậy. Để tránh việc này, lập trình viên cần phải đặt mình vào góc nhìn của người đọc và để tâm đến việc refactor làm sao cho code của họ dễ hiểu nhất có thể. Đồng thời, code luôn cần được review bởi người khác để đảm bảo nó dễ hiểu.

#### Thế tại sao phần mềm lại trở nên xuống cấp?

Trong môi trường không agile, thiết kế xuống cấp xảy ra là do yêu cầu của phần mềm thay đổi mà thiết kế ban đầu không lường trước. Thông thường, những thay đổi này cần được triển khai nhanh, và chúng thường được thực hiện bởi những lập trình viên không nắm rõ triết lý của thiết kế ban đầu. Vì thế, mặc dù những thay đổi này vẫn đáp ứng được thay đổi về yêu cầu, chúng lại vi phạm tầm nhìn của thiết kế ban đầu. Từng chút một, khi các thay đổi diễn ra, nguyên tắc của thiết kế ban đầu càng bị vi phạm, và thiết kế dẫn trở nên bốc mùi.

Tuy vậy, ta không thể nào đổ lỗi của việc phần mềm xuống cấp cho việc thay đổi yêu cầu. Chúng ta với tư cách là các lập trình viên đều hiểu rõ rằng yêu cầu chính là thứ hay thay đổi nhất trong quá trình phát triển phần mềm. Nếu mà thiết kế cũng chúng ta xuống cấp dần theo mỗi thay đổi của yêu cầu thì đó là lỗi ở thiết kế hay là quy trình phát triển của ta, và ta cần tìm cách làm sao cho thiết kế của mình bền vững trước những thay đổi như vậy.

#### Một đội tuân theo Agile sẽ không để phần mềm của mình xuống cấp

Một đội ngũ Agile hoan nghênh những thay đổi. Họ đặt cược rất ít vào ban đầu, và vì vậy, họ cũng không có nhiều động lực phải tuân theo một thiết kế ban đầu không còn phù hợp. Thay vì có một thiết kế ban đầu, họ làm thế nào để cho phần mềm sạch sẽ và đơn giản nhất có thể, đồng thời kèm theo trong phần mềm của mình rất nhiều unit test và test chấp nhận. Việc này làm cho thiết kế trở nên linh hoạt hơn, và đội ngũ agile sẽ tận dụng sự linh hoạt đó để liên tục cải tiến thiết kế phần mềm sao cho ở cuối mỗi iteration, phần mềm thu được là phù hợp nhất với yêu cầu hiện tại.
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

### Ứng dụng `Shape`

Ứng dụng `Shape` nổi tiếng dưới đây đã được nhắc đi nhắc lại trong vô số cuốn sách về thiết kế hướng đối tượng. Nó thường được dùng để thể hiện cách mà tính đa hình hoạt động. Tuy vậy, bây giờ ta sẽ dùng nó để ví dụ cho OCP.

Ta có một phần mềm cần phải vẽ các hình vuông và hình tròn lên một GUI. Các hình vuông và hình tròn cần được vẽ theo một thứ tự nhất định. Một list các hình vuông cà tròn theo thứ tự sẽ được cấp, chương trình phải đọc list đó và tuần tự vẽ hình ở từng bước.

#### Vi phạm OCP

Ngôn ngữ C sử dụng phương pháp lập trình thủ tục không tuân thủ OCP để xử lý vấn đề này. Ta có thể xử lý vấn đề này như cách làm trong *snippet 9-1* . Ở đây ta có một cấu trúc dự liệu có chung phần tử đầu tiên là code chỉ loại và còn lại thì khác nhau. Hàm `DrawAllShape` sẽ duyệt qua mảng các con trỏ chỉ đến cấu trúc dữ liệu này, dựa vào code loại để gọi đúng hàm vẽ hình vuông hay hình tròn.

```c
--shape.h---------------------------------------
enum ShapeType {circle, square};
struct Shape
{
 ShapeType itsType;
};
--circle.h---------------------------------------
struct Circle
{
    ShapeType itsType;
    double itsRadius;
    Point itsCenter;
};
void DrawCircle(struct Circle*);
--square.h---------------------------------------
struct Square
{
    ShapeType itsType;
    double itsSide;
    Point itsTopLeft;
};
void DrawSquare(struct Square*);
--drawAllShapes.cc-------------------------------
typedef struct Shape *ShapePointer;
void DrawAllShapes(ShapePointer list[], int n)
{
    int i;
    for (i=0; i<n; i++)
 {
    struct Shape* s = list[i];
    switch (s->itsType)
    {
        case square:
            DrawSquare((struct Square*)s);
        break;
        case circle:
            DrawCircle((struct Circle*)s);
        break;
    }
 }
}

```
*Snippet 9-1*

Hàm `DrawAllShape` không tuân thủ OCP vì nó không thể đóng với những loại hình mới. Nếu tôi muốn mở rộng phần mềm này để có thể vẽ được cả tam giác, tôi sẽ không thể làm gì khác ngoài việc chỉnh sửa hàm này. Thực tế là tôi sẽ phải sửa hàm này mỗi lần tôi muốn vẽ được thêm một loại hình.

Tất nhiên, chương trình này chỉ là một ví dụ đơn giản. Trong thực tế, câu lệnh `switch` trong `DrawAllShape` sẽ cần phải được lặp đi lặp lại ở vô số nơi, mỗi nơi lại khác đi một ít, trong khắp chương trình. Sẽ có những hàm thực hiện kéo hình, dời hình, xóa hình, ... Việc thêm một loại hình vào chương trình này đồng nghĩa với việc phải tìm toàn bộ những nơi sử dụng các câu lệnh `switch` hay `if/else` và thêm hình mới ở đó.

Hơn thế nữa, điều kiện của các câu lệnh `if/else` này có thể sẽ không đẹp và đơn giản như câu lệnh `switch` trong `DrawAllShape`. Thực tế là ở mỗi câu lệnh điều kiện, khả năng cao là chúng sẽ được kết hợp với những điều kiện khác để làm cho việc quyết định hành động ở khu vực đó trở nên dễ hơn, hay là các câu lệnh `case` sẽ được kết hợp để "đơn giản hóa" việc quyết định hành động. Thậm chí, sẽ có cả những phần công việc giống nhau cho cả hình vuông và hình tròn, và ở những khu vực sử dụng chúng sẽ thậm chí không có câu lệnh `if/else` hay `switch/case`. Vì những vấn đề kể trên, việc tìm ra tất cả những nơi mà hình mới cần được thêm vào là không hề đơn giản.

Chưa hết, xem xét tới loại thay đổi cần phải được thực hiện. Vì ta sẽ phải thêm một member vào enum ShapeType, ta sẽ phải compile lại nó. Những loại hình khác đều phụ thuộc vào enum này, khi enum này được compile lại, ta cũng phải compile lại tất cả những class hình khác. Và rồi ta cũng sẽ phải compile lại tất cả các module phụ thuộc vào `Shape`.

Vậy là ta không chỉ cần phải sửa source code ở những nói dùng `if/else` và `switch/case`, ta còn phải chỉnh sửa tất cả các file binary của toàn bộ modules dùng bất kỳ loại `Shape` nào. Thay đổi binary đồng nghĩa với việc toàn bộ DDLs, shared library, hay thành phần binary đều phải được redeploy. Ta có thể dễ dàng nhận thấy hệ quả của việc thêm chỉ một `Shape` là vô cùng lớn khi mà nó buộc ta phải thay đổi rất nhiều source và binary modules khác nhau.

**Thiết kệ tệ** - Vậy nói tóm lại, thiết kế trên ở *snippet 9-1* là một thiết kế tệ. Nó *cứng nhắc* vì việc thêm một `Triangle` vào sẽ buộc việc phải compile và deploy lại toàn bộ `Shape`, `Square`, `Circle`, và `DrawAllShape`. Nó *dễ vỡ* vì sẽ có vô số những câu lệnh `if/else` và `switch/case` rất khó để tìm thấy cũng như hiểu. Nó *ục ịch* vì nếu muốn tái sử dụng `DrawAllShape` ở một chương trình khác, ta phải mang theo cả `Square` và `Circle`mặc dù chương trình đó không cần chúng. Rõ ràng, thiết kế này chứa đầy dấu hiệu cảu  một thiết kế tệ.

#### Tuân thủ OCP

*Snippet 9-2* là một giải pháp tuân thủ OCP. Trong trường hợp này, ta tạo ra một lớp trừu tượng `Shape`. Lớp trừu tượng này chỉ có một hàm trừu tượng là `Draw`. Cả `Square` và `Circle` đều sẽ được mở rộng từ `Shape`.

```cpp
class Shape
{
    public:
        virtual void Draw() const = 0;
};
class Square : public Shape
{
    public:
        virtual void Draw() const;
};
class Circle : public Shape
{
    public:
        virtual void Draw() const;
};
void DrawAllShapes(vector<Shape*>& list)
{
    vector<Shape*>::iterator i;
    for (i=list.begin(); i != list.end(); i++)
        (*i)->Draw();
}
```

Để ý rằng nếu ta muốn mở rộng hành vi của `DrawAllShape` ở `snippet 9-2` để vẽ một hình khác, tất cả những gì ta cần làm là thêm một lớp mở rộng từ `Shape`. `DrawAllShape` chẳng cần thay đổi gì cả. Vì thế `DrawAllShape` tuân thủ OCP. Hành vi của nó có thể được mở rộng mà không cần phải trực tiếp thay đổi source code. Thực tế, việc thêm class `Triangle` chẳng gây ảnh hưởng gì đến bất kỳ module nào ở đây. Tất nhiên, sẽ có những phần của phần mềm sẽ cần thay đổi để xử lý `Triangle`, nhưng phần code ở đây hoàn toàn miễn nhiễm với việc này.

Trong thực tế, lớp `Shape` sẽ có nhiều hàm hơn thế này. Tuy vậy, việc thêm một hình mới vào sẽ tương đối đơn giản vì ta sẽ chỉ cần viết một lớp mở rộng cho `Shape` và cụ thể hóa tất cả các hàm cho nó. Ta chẳng cần phải đi lùng sục khắp phần mềm để tìm tất cả những nơi cần thay đổi, và vì thế, phần mêm này không hề *dễ vỡ*.

Thiết kế này cũng không cứng nhắc. Chẳng có module hiện hữu nào cần phải bị thay đổi cả, và trừ module phải tạo ra các hình cụ thể, không module nào cần phải được compile lại cả. Thông thường những thay đổi này sẽ được thực hiện ở `main`, ở một vài hàm được gọi bởi `main`, hoặc là ở phương thức của một vài đối tượng được tạo bơi `main`.

Cuối cùng, phương pháp này không *ục ịch*. Ta có thể tái sử dụng `DrawAllShape` ở bất cứ đâu mà không cần mang theo `Square` hay `Circle`. Như vậy, có thể thấy, thiết kế này hoàn toàn không có các dấu hiệu của thiết kế tệ liệt kê trước đó.

Thiết kế này tuân thủ OCP. Nó thay đổi bằng cách thêm code mới thay vì chỉnh sửa code cũ. Bằng cách này nó không phải lãnh chịu việc hệ quả dây chuyền mà những phần mềm không tuân thủ OCP phải chịu

#### Ừ thì cũng không hẳn

Ví dụ vừa rồi chỉ là trường hợp lý tưởng! Tưởng tượng chuyện gì sẽ xảy ra với `DrawAllShape` ở *snippet 9-2* nếu ta quyết định rằng tất cả hình tròn phải được vẽ *trước* hình vuông? Hàm `DrawAllShape` không đóng với thay đổi như thế này! Để làm được điều này, ta cần phải scan danh sách các hình và lấy ra tất cả `Circle`, và sau đó cho `Square`.

#### Dự đoán và cấu trúc "Tự nhiên"

Nếu ta có thể đoán trước và chuẩn bị cho thay đổi như thế này, ta có thể nghĩ ra một việc trừu tượng hóa có thể bảo vệ ta trước thay dổi này. Cách trừu tượng hóa ở *snippet 9-2* bây giờ lại thành một chướng ngại cho ta trong việc thích nghi trước thay đổi này. Điều này có thể gây ngạc nhiên. Chẳng phải việc `Shape` là base class với `Square` và `Circle` là các lớp mở rộng là tự nhiên nhất rồi hay sao? Chẳng phải cấu trúc tự nhiên nhất là cấu trúc nên dùng nhất sao? Câu trả lời nằm ở việc mô hình này *không* tự nhiên trong một môi trường mà thứ tự quan trọng hơn loại hình.

Điều này dẫn ta đến một kết luận đáng ngại: Tổng quát mà nói, cho dù một mmoo hình có đóng đén mức nào, vẫn có những thay đổi mà ta bị buộc phải mở nó. *Không cớ mô hình nào là hoàn hảo trong mọi trường hợp.*

Bởi vì ta không thể đóng mô hình của mình hoàn toàn, ta cần đóng nó một cách có chọn lọc. Điều này có nghĩa là người thiết kế hệ thông cần phải chọn những thay đổi mà hệ thống của họ có thể bảo toàn trước những thay đổi đó.

Điều này cần một lượng kinh nghiệm. Một kiến trúc sư đủ dày dặn kinh nghiệm mong rằng họ hiểu người dùng cũng như thị trường của sản phẩm của mình đủ tốt để có thể đánh giá xác suất xảy ra của mỗi thay đổi và sau đó ứng dụng OCP cho những thay đổi có xác suất cao nhất.

Hơn thế nữa, tuân thủ OCP rất phức tạp và mất công. Nó yêu cầu người thiết kế phải bỏ thời gian để nghiên cứu và sáng tạo ra những cấu trúc trừu tượng phù hợp. Không chỉ thế, việc làm việc với những cấu trúc trừu tượng cũng làm cho việc phát triển phần mềm trở nên phức tạp hơn, và mỗi lập trình viên cũng có một giới hạn về độ trừu tượng mà họ có thể làm việc được. Có thể thấy, có nhiều lý do để ta muốn  chỉ apply OCP cho những thay đổi có khả năng xảy ra cao.

Vậy làm sao ta biết được thay đổi nào có khả năng xảy ra cao? Ta chỉ có thể thực hiện các nghiên cứu phù hợp, đặt những câu hỏi phù hợp, và vận dụng kinh nghiệm cũng như nghĩ theo lẽ thường. Và sau đó, ta chỉ có cách là chờ cho đến lúc thay đổi xảy ra.

#### Cài sẵn móc

Làm sao để ta bảo vệ bản thân truốc những thay đổi? Trước đây, có một câu nói rằng ta sẽ cài sẵn những cái "móc" để chuẩn bị cho những thay đổi mà có thể xảy ra với niềm tim rằng việc làm vậy sẽ khiến phần mềm trở nên mềm dẻo hơn.

Tuy vậy, những cái móc ta cài sẵn lại thường không đúng. Tệ hơn nữa, những cái móc này bốc mùi *phức tạp thừa thãi*, và ta sẽ phải bảo quản và hỗ trợ chúng mặc dù chúng chẳng giúp gì cho ta. Việc này cũng không tốt, ta không muốn phải tay xách nách mang đủ thứ trừ tượng hóa không cần thiết. Thay vì vậy, điều tốt hơn nên làm là việc chờ đến khi ta thực sự cần một sự trừu tượng rồi hẵng thêm nó vào. 

**Bị dụ một lần** - Cổ ngữ có cầu: "Bạn dụ tôi một lần là lỗi của bạn, bạn dụ tôi hai lần là lỗi của tôi." Đây là một thái độ đúng đắn trong việc phát triển phần mềm. Để không phải đưa vào phần mềm của ta đủ thứ lỉnh kỉnh, ta chấp nhận để bản thân mình bị lừa một lần. Nghĩa là đầu tiên, khi ta lập trình ta sẽ làm như thể nó sẽ không cần phải thay đổi gì. Khi thay đổi xảy ra, ta sẽ thêm vào phần mềm cơ chế để chống lại *tất cả* thay đổi *tương tự*. Nói khác đi, ta sẽ chấp nhận ăn đòn một lần, và rồi tự bảo về mình trước tất cả những đòn đến từ cùng một nguồn.

**Giả lập thay đổi** - Nếu ta chấp nhận rằng mình sẽ bị ăn đòn (ít nhât là đòn đầu tiên), ta muốn những đòn này đến càng sớm càng tốt. Chúng ta muốn biết càng sớm càng tốt những thay đổi có thể xảy ra cho phần mềm của mình. Càng đợi lâu, việc thay đổi càng trờ nên khó khăn và càng có nhiều ảnh hưởng.

Chính vì thế, ta cần giả lập và chuẩn bị cho những thay đổi cso thể xảy ra. Ta sẽ làm việc này bằng nhiều cách như được liệt kê ở chương 2:

- Viết test trước. Testing là một dạng sử dụng hệ thống. Bằng cách viết test trước, ta bắt hệ thống phải trở nên test được. Bằng cách này, thay đổi về mặt test sẽ không làm ta bất ngờ sau này.
- Phát triển theo những giai đoạn ngắn - tính bằng ngày thay vì tuần.
- Phát triển chức năng trước kiến trúc, và cho các bên liên quan được trải nghiệm những tính năng được phát triển.
- Phát triển các tính năng quan trọng trước. 
- Giao sản phẩm sớm và liên tục. Ta đưa sản phẩm của mình đến khách hàng và người dùng nhanh và thường xuyên nhất có thể.

#### Tận dụng trừu tượng hóa để đạt được sự đóng rõ ràng

Rồi, ta đã nhận một đòn đau. Người dùng bây giờ muốn ta vẽ hình tròn trước hình vuông. Bây giờ ta muốn bảo vệ bản thân khỏi bất cứ thay đổi nào tương tự trong tương lai.

Làm sao để ta đóng phần mềm trước bất kỳ thay đổi nào về thứ tự của các hình? Nhớ rằng việc đóng của phần mềm luôn được dựa trên trừu tượng hóa. Vì vậy, nếu ta muốn đạt được tính đóng với thay đổi về thứ tự, ta cần có một trừu tượng hóa về thứ tự. Sự trừu tượng hóa này sẽ cho phép ta diễn tả bất kỳ chính sách thứ tự mong muốn nào thông qua một interface.

Một chính sách thứ tự có nghia là với 2 vật thể bất kỳ ta có thể biết được là cần vẽ vật nào trước vật nào sau. Ta  có thể làm việc này bằng cách tạo một phương thức trừu tượng `Precede` trong `Shape` mà nó nhận vào một con trỏ đến một đối tượng `Shape`, trả về `true` nếu như hình hiện tại cần được vẽ trước hình được con trỏ đưa vào trỏ đến và `false` trong trường hợp ngược lại.

Trong `C++`, điều này cũng có thể được diễn tả bằng việc overload dấu `<`. *Snippet 9-3* thể hiện lớp `Shape` sau khi ta thêm phương thức so sánh.

Với việc đã có được cách để biết hình nào cần được vẽ trước và hình nào cần được vẽ sau, ta có thể thực hiện sắp xếp chúng trước khi vẽ chúng như *snippet 9-4*.

```cpp
class Shape
{
    public:
        virtual void Draw() const = 0;
        virtual bool Precedes(const Shape&) const = 0;
        bool operator<(const Shape& s) {return Precedes(s);}
};
```
*Snippet 9-3*

```cpp
template <typename P>
class Lessp // utility for sorting containers of pointers.
{
    public:
        bool operator()(const P p, const P q) {return (*p) < (*q);}
};
void DrawAllShapes(vector<Shape*>& list)
{
    vector<Shape*> orderedList = list;

    sort(orderedList.begin(),
    orderedList.end(),
    Lessp<Shape*>());

    vector<Shape*>::const_iterator i;
    for (i=orderedList.begin(); i != orderedList.end(); i++)
        (*i)->Draw();
}
```
*Snippet 9-4*

Việc này cho ta một cách để sắp xếp các đối tượng `Shape` và vẽ chúng theo đúng thứ tự. Tuy vậy, ta vẫn chưa có một trừu tượng hóa thứ tự rõ ràng. Như hiện tại, mỗi lớp con của shape vẫn cần override hàm `Precedes` để thể hiện thứ tự của mình. Xem xét đến code cần được viết cho `Circle::Precedes` để đảm bảo hình tròn được vẽ trước hình vuông. (*Snippet 9-5*)

```cpp
bool Circle::Precedes(const Shape& s) const
{
    if (dynamic_cast<Square*>(s))
        return true;
    else
        return false;
}
```
Rõ ràng rằng hàm này cũng như tất cả các hàm tương tự ở các lớp con khác của `Shape` không hề đóng. Chẳng có cách nào để đóng nó cho tất cả các mở rộng của `Shape`. Mỗi lần một `Shape` được tạo ra, tất cả các hàm `Precedes` phải được chỉnh sửa.

Tất nhiên, điều này cũng không quan trọng nếu không có bất cứ hình nào mới được thêm vào. Mặt khác, nếu các loại `Shape` khác cứ liên tục được thêm vào, thiết kế này sẽ gây ra một hậu quả rất lớn. Một lần nữa, ta lại nhận cú đầu tiên.

#### Sử dụng phương pháp "hướng dữ liệu" để đạt được tính đóng

Nếu ta cần phải đóng các mở rộng của `Shape` với sự tồn tại của nhau, ta có thể áp dụng một bảng thứ tự. *Snippet 9-6* là một hướng tiếp cận.
```cpp
#include <typeinfo>
#include <string>
#include <iostream>
using namespace std;
class Shape
{
    public:
        virtual void Draw() const = 0;
        bool Precedes(const Shape&) const;
        bool operator<(const Shape& s) const
        {return Precedes(s);}
    private:
        static const char* typeOrderTable[];
};
const char* Shape::typeOrderTable[] =
{
    typeid(Circle).name(),
    typeid(Square).name(),
    0
};
// This function searches a table for the class names.
// The table defines the order in which the
// shapes are to be drawn. Shapes that are not
// found always precede shapes that are found.
//
bool Shape::Precedes(const Shape& s) const
{
    const char* thisType = typeid(*this).name();
    const char* argType = typeid(s).name();
    bool done = false;
    int thisOrd = -1;
    int argOrd = -1;
    for (int i=0; !done; i++)
    {
        const char* tableEntry = typeOrderTable[i];
        if (tableEntry != 0)
        {
            if (strcmp(tableEntry, thisType) == 0)
                thisOrd = i;
            if (strcmp(tableEntry, argType) == 0)
                argOrd = i;
            if ((argOrd >= 0) && (thisOrd >= 0))
                done = true;
        }
        else // table entry == 0
            done = true;
    }
    return thisOrd < argOrd;
}
```
*Snippet 9-6*

Bằng cách tiếp cận này, ta đã có thể đóng `DrawAllShapes` với mọi thay đổi về thứ tự nói chung. Ta cũng đóng các mở rộng của `Shape` đối với việc có thêm những mở rộng mới hay là việc thứ tự mong muốn bị thay đổi.

Thứ duy nhất không đóng ở đây trước những loại thay đổi này là bảng thứ tự. Bảng đó có thể được đặt tách ra ở một module riêng, tách biệt khỏi các module `Shape` để cho những thay đổi của nó không gây ảnh hưởng đến các modules này. Trong C++, ta thậm chí có thể chọn sử dụng bảng nào ở
thời điểm link (link time).

### Kết luận 
Theo nhiều cách, OCP chính là trái tim của lập trình hướng đối tượng. Việc tuân thủ nguyên lý này là chìa khóa để đạt được những tính chất được hứa hẹn bởi lập trình hướng đối tượng. Tuy vậy, việc tuân thủ nguyên tắc này không phải đạt được chỉ bằng việc dùng ngôn ngữ lập trình hướng đối tượng. Đồng thời, ta cũng không nên mù quáng áp dụng trừu tượng hóa cho tất cả mọi phần của phần mềm mà thay vào đó, lập trình viên cần phải biết ứng dụng nó phù hợp cho những phần thường xuyên phải thay đổi trong phần mêm. *Chống lại trừu tượng hóa quá sớm cũng quan trọng như là việc trừu tượng hóa vậy*.

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

### Một ví dụ thực tế

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

![Figure 11-1](./imgs/Figure%2011-1.png)

*Hình 11-2* là một thiết kế cải tiến hơn. Mỗi tầng sẽ khai báo một bộ các interface trừu tượng để diễn tả những service mà nó cần dùng. Sau đó, tầng thấp hơn sẽ hiện thực hóa các interface trừu tượng ở tầng ngay trên của nó. bằng cách này, các lớp thuộc tầng trên sẽ không phụ thuộc vào các lớp ở tầng dưới mà thay vào đó sẽ phụ thuộc và các lớp trừu tưởng ở cùng tầng. Như vậy, không chỉ sự phụ thuộc của `PolicyLayer` vào `UtilityLayer` bị phá vỡ, mà cả sự phụ thuộc của `PolicyLayer` vào `MechanismLayer` cũng bị bẻ gãy.

![Figure 11-2](./imgs/Figure%2011-2.png)

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

![Figure 11-3](./imgs/Figure%2011-3.png)

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
*Snippet 11-1*

Thiết kế này vi phạm DIP, các chính sách ở tầng cao vẫn chưa được tách bạch khỏi các triển khai ở tầng thấp. Những khái niệm trừu tượng vẫn chưa được tách ra khỏi những triển khai cụ thể. Nếu không có sự tách biệt này, những modules ở tầng cao sẽ nghiễm nhiên phải phụ thuộc vào các triển khai cụ thể ở dưới thấp, và sự trừu tượng chắc chắn phải phụ thuộc lên những triển khai cụ thể.

#### Tìm kiếm sự trừu tượng bên dưới
Thế nào là chính sách cấp cao? Nó là sự trừu tượng đằng sau của chương trình, sự thật không thay đổi khi mà các chi tiết cơ chế thay đổi. Nó là một hệ thống nằm ở trong hệ thống của chương trình. Trong ví dụ về `Button/Lamp`, sự trừu tượng bên dưới là việc cảm nhận một tín hiệu bật tắt từ người dùng và chuyển tiếp tín hiệu đó đến một đối tượng khác. Cơ chế để phát hiện tín hiệu từ người dùng hay vật thể đích hoàn toàn không quan trong ở đây. 

Design ở *Hình 11-3* có thể được cải tiến bằng cách đảo ngược sự phụ thuộc của đối tượng `Lamp`. Ở *Hình 11-4*, ta thấy rằng `Button` bây giờ sẽ liên kết với một vật trừu tượng gọi là `ButtonServer`. `ButtonServer` cung cấp những hàm trừu tượng mà `Button` có thể sử dụng để bật hay tắt một vật gì đó. `Lamp` bây giờ sẽ cụ thể hóa `ButtonServer` và sẽ thực hiện sự phụ thuộc lên thay vì bi phụ thuộc vào.

![Figure 11-4](./imgs/Figure%2011-4.png)

Thiết kế ở *Hình 11-4* cho phép `Button`  điều khiển bất kỳ đối tượng nào cụ thể hóa `ButtonServer` interface. Việc này cho ta phép ta linh hoạt ứng dụng `Button` cho cả những đối tượng khác chưa được tạo ra. 

Tuy vậy, solution này vẫn đặt ra một yêu cầu lên các đối tượng muốn được điều khiển bởi Button. Những đối tượng này buộc phải triển khai `ButtonServer`. Điều này quả là đáng tiếc vì những đối tượng này cũng có thể mong muốn được điều khiển các loại điều khiển khác ví dụ như `Switch`. 

Mặc dù ta đã đảo ngược sự phụ thuộc và để `Lamp` thực hiện phụ thuộc thay vì là bị phụ thuộc lên, ta buộc `Lamp` phải phụ thuộc vào một chi tiết khác - `Button`. Hay là không?

`Lamp` chắc chắn là phụ thuộc lên `ButtonServer`, nhưng `ButtonServer` lại không hề phụ thuộc vào `Button`. Bất kỳ lớp nào biết cách điều khiển `ButtonServer` đều có thể được dùng để điều khiển `Lamp`. Vậy nên sự phụ thuộc của `ButtonServer` lên `Button` thực chất chỉ là ở cái tên. Và ta có thể gỡ bỏ nó đơn giản bằng cách đổi tên `ButtonServer` thành một cái tên chung chung hơn như là `SwitchableDevice`. Ta cũng có thể tách `Button` và `SwitchableDevice` vào các thư viện khác nhau để đảm bảo rằng việc sử dụng `SwitchableDevice` không ám chỉ việc sử dụng `Button`.

Vậy là trong trường hợp này, ta thu được một kết quả vô cùng thú vị là chẳng ai sở hữu interface `SwitchableDevice`. Interface này có thể được dùng bởi vô số các client khác nhau, cũng như có thể được cụ thế hóa bởi vô số các server khác nhau. Vì vậy, interface này nên được tách biệt ra khỏi cả nhóm các client và nhóm các server. Trong C++, ta sẽ đặt interface này vào một library và namespace riêng. Trong Java, điều này có nghĩa là ta sẽ đặt interface này vào một package riêng.

### Ví dụ về `Furnace`
### Kết luận

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
*Snippet 12-1*

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
*Snippet 12-2*

Khi một vật muốn được thông báo về việc time-out, nó sẽ gọi đến hàm `Register` của timer để đăng ký. Trong đó argument `timeout` của hàm `Register` là chỉ thời gian cảu việc time-out, và nó sẽ cung cấp thêm một con trỏ đến một vật `TimerClient` mà khi đến lúc nó sẽ gọi hàm `Timeout` cho vật đó.

Vậy làm thế nào ta có thể làm cho `TimerClient` liên hệ đến `TimedDoor` để code ở trong `TimedDoor` có thể được thông báo khi đến lúc time-out? Có một vài cách khác nhau. Hình *12-1* là một cách làm ngây thơ. Ta bắt `Door`, và từ đó dẫn đến `TimedDoor`, kế thừa từ `TimerClient`. Điều này đảm bảo rằng `TimerClient` có thể được đăng ký với `Timer` và nhận được tin nhắn `TimeOut`.

![Figure 12-1](./imgs/Figure%2012-1.png)

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
*Snippet 12-3*

Rõ ràng thay đổi này sẽ gây ảnh hưởng tới tất cả các lớp phụ thuộc vào *TimerClient*. Ta chấp nhận việc này vì việc không có `timeOutId` là một thiếu sót trong quá trình thiết kế `Timer` cần phải được sửa đổi. Tuy vậy, thiết kế ở *snippet 12-1* buộc `Door` và tất cả các lớp phụ thuộc vào `Door` bị vạ lây. Đây là một dấu hiệu rõ ràng của tính nhớt và tính cứng nhắc. Tại sao một bug ở `TimerClient` lại gây ảnh hưởng lên những lớp phụ thuộc vào các lớp con không cần hẹn giờ của `Door`. Khi sự thay đổi ở một phần của phần mềm gây ảnh hưởng lên một phần khác chẳng hề liên quan trong chương trình, hậu quả của những thay đổi này trở nên khó đoán, và nguy cơ vỡ vụn trước thay đổi của chương trình trở nên rất cao.

### ISP: Nguyên lý rạch ròi interface

> Các khách hàng không nên bị ép phải phụ thuộc vào những hàm mà họ không dùng đến.

Khi mà các lớp khách hàng phải phụ thuộc lên những hàm họ không dùng đến họ sẽ bị ảnh hưởng bởi các thay đổi tiềm tàng của các hàm này. Điều này dẫn đến sự ràng buộc không đoán trước được giữa các lớp phụ thuộc. Nói khác đi, khi một client phụ thuộc vào một hàm nó không dùng đến nhưng được dùng bởi một lớp khác, nó sẽ chịu ảnh hưởng bởi những thay đổi lên hàm đó do những lớp dùng hàm đó gây ra. Ta muốn tránh những sự ràng buộc này bất kỳ lúc nào có thể, vì vậy nên ta cần sự rạch ròi giữa các interface.

### Interface của lớp v. interface của đối tượng

Lại xét `TimedDoor`. Đây là một đối tượng cần phục vụ 2 nhóm khách hàng khác nhau-`Timer` và `Door`. Cả 2 interface này cần phải được triển khai bởi cùng một đối tượng bên dưới vì chúng cùng tương tác với một bộ dữ liệu chung. Vậy làm sao để ta tuân thủ ISP? Làm sao để ta làm các interface trở nên rạch ròi cho dù chúng vẫn phải nằm cùng nhau?

Câu trả lời cho việc này nằm ở việc là các client không cần phải tương tác trực tiếp với các hàm do interface của đối tượng trưng ra. Nó có thể tương tác với những hàm này thông qua việc ủy quyền hay thông qua một lớp cha của đối tượng.

#### Phân tách thông qua ủy quyền

Một cách giải quyết cho vấn đề ở trên là việc tạo ra một đối tượng cụ thể hóa `TimerClient` và điều hướng tín hiệu đến cho `TimedDoor`. *Hình 12-2* thể hiện cách làm này.

![Figure 12-2](./imgs/Figure%2012-2.png)

Khi `TimedDoor` muốn đăng ký một timeout với `Timer`, nó sẽ tạo một đối tượng `DoorTimerAdapter` và đăng ký đối tượng này với `Timer`. Khi `Timer` gửi tín hiệu `TimeOut` đến cho `DoorTimerAdapter`, nó sẽ chuyển tiếp tín hiệu này qua cho `TimedDoor`.

Việc làm này tuân thủ ISP và năng chặn việc ràng buộc giữa các `Door` clients và `Timer`. Kể cả khi những thay đổi ở *snippet 12-3* xảy ra cho `Timer`, các đối tượng phụ thuộc vào `Door` cũng chẳng hề hấn gì. Hơn thế nữa, TimedDoor cũng chẳng cần phải tuẩn thủ interface `TimerClient` do `Timer` định nghĩa ra khiến solution này trở vô cùng đa dụng.

```cpp
class TimedDoor : public Door
{
    public:
        virtual void DoorTimeOut(int timeOutId);
};

class DoorTimerAdapter : public TimerClient
{
    public:
        DoorTimerAdapter(TimedDoor& theDoor): itsTimedDoor(theDoor){}
        virtual void TimeOut(int timeOutId)
        {itsTimedDoor.DoorTimeOut(timeOutId);}
    private:
        TimedDoor& itsTimedDoor;
};
```
*Snippet 12-4*

Tuy vậy, solution này vẫn có phần thiếu tinh tế. Nó yêu cầu việc tạo ra một đối tượng mới mỗi khi cần đăng ký một timeout. Hơn thế nữa, việc chuyển tiếp tín hiệu vẫn cần một lượng, dù rất nhỏ, run-time và memory. Có những ứng dụng, ví dụ như trong những bộ điều khiển embedded thời gian thực, mà lượng run-time và memory cần dùng cho việc này là một vấn đề.

#### Tạo sự minh bạch thông qua đa kế thừa

*Hình 12-3* và *snippet 12-5* mô tả một cách giải quyết khác cho vấn đề này dựa vào đa kế thừa. Trong mô hình này, `TimedDoor` kế thừa và triển khai đồng thời cả 2 interface `Door` và `TimerClient`. Mặc dù các client dùng mỗi lớp gốc đều có thể được dùng để điều khiển `TimedDoor`, không có bất kỳ client nào phụ thuộc vào `TimedDoor`. Như vậy, client của cả 2 lớp trừu tượng đều có thể tương tác với một đối tượng `TimedDoor` qua các interface riêng biệt.

![Figure 12-3](./imgs/Figure%2012-3.png)

```cpp
class TimedDoor : public Door, public TimerClient
{
    public:
        virtual void TimeOut(int timeOutId);
};
```
*Snippet 12-5*

Đây là cách làm được khuyến nghị bởi tác giả. Tác giả chỉ ưu tiên phương pháp ở *Hình 12-2* nếu việc chuyển đổi thực hiện bởi `DoorTimerAdapter` là không thể tránh khỏi, hoặc là cần nhiều kiểu chuyển đổi khác nhau vao những thời điểm khác nhau.

### Ví dụ về ATM User Interface

Bây giờ hãy cùng xem xét một ví dụ thú vị hơn. Vấn đề cơ bản của một máy ATM truyền thống. UI của một máy ATM truyền thống cần phải rất linh hoạt, đầu ra cần phải được thể hiện được bằng nhiều ngôn ngữ, có khi thì đầu ra này cần được thể hiện qua một màn hình, lúc khác thì nó lại cần được đọc lên, một thời điểm khác nữa nó lại cần được thể hiện trên một bảng braille. Rõ ràng, ta có thể đạt được điều này bằng cách dùng một interface trừu tượng với các hàm trừu tượng thể hiện các loại tin nhắn cần được gửi với UI này.

![Figure 12-4](./imgs/Figure%2012-4.png)

Đồng thời, các loại giao dịch khác nhau có thể được thực hiện bởi máy ATM này cũng được cụ thể hóa thông qua một mở rộng của interface trừu tượng `Transaction`. Như vậy, ta có thể sẽ có các lớp như `DepositTransaction`, `WithdrawalTransaction`, và `TransferTransaction` mà mỗi lớp này lại sẽ gọi đến những hàm được cung cấp của UI. Ví dụ `DepositTransaction` sẽ gọi `RequestDepositAmount` để biết được người dùng muốn gửi bao nhiêu tiền. Tương tự vậy, `TransferTransaction` sẽ gọi `RequestTransferAmount` để xem người dùng muốn chuyển bao nhiêu tiền giữa các tài khoản. Điều này được thể hiện ở diagram 12-5.

![Figure 12-5](./imgs/Figure%2012-5.png)
***Hình 12-5.** Cấu trúc kế thừa của giao dịch ATM*

Chú ý rằng đây chính xác là trường hợp mà ISP khuyên chúng ta nên tránh. Mỗi loại giao dịch chỉ dùng một vài phương thức của UI mà các loại giao dịch khác không dùng đến. Điều này tạo ra một khả năng rõ ràng là nếu có một thay đổi nào đó ở một transaction buộc UI phải thay đổi, tất cả các transaction cũng như các lớp phụ thuộc lên chúng phải được compile lại. Có một dấu hiệu dễ vỡ đâu đây!

Cụ thể hơn, ví dụ như có một `PayGasBillTransaction` được thêm vào, ta sẽ cần chỉnh sửa interface UI để thêm các hàm hỗ trợ cho loại giao dịch mới này. Đáng tiếc thay, bởi vì `DepositTransaction`, `WithdrawalTransaction`, và `TransferTransaction` đều phụ thuộc lên UI, tất cả đều phải được compile lại. Tệ hơn nữa, nếu các loại giao dịch này được deploy ở các DDL hay thư viện khác nhau, tất cả các DDL và thư viện này sẽ đều phải được deploy lại mặc dù chẳng có logic gì ở trong chúng bị thay đổi cả. Đã cảm thấy nó nhớp nháp chưa?

May mắn thay, sự ràng buộc không đáng có này có thể dễ bị phá vỡ bằng cách phân rã interface UI thành nhiều interface riêng lẻ cho mỗi loại giao dịch (`DepositUI`, `WithdrawalUI`, và `TransferUI`). Các interface này sau đó có thể được đa kế thừa bởi interface UI. *Hình 12-6* và *Snippet 12-6* thể hiện thiết kế này.

![Figure 12-6](./imgs/Figure%2012-6.png)
***Hình 12-6.** Cấu trúc kế thừa của giao dịch ATM đã được phân tách*

Mỗi khi ta muốn thêm một loại giao dịch mới mở rộng từ `Transaction`, một UI tương ứng cho nó phải được tạo ra. Điều này buộc ta phải thay đổi UI interface cũng như các lớp con của nó. Tuy nhiên, các lớp này lại không có quá nhiều lớp phụ thuộc lên chúng. Thực tế là chúng chỉ được dùng bởi `main` hay là một process nào đó chịu trách nhiệm khởi động hệ thống và tạo các các đối tượng cụ thể của UI, do đó việc thay đổi trực tiếp vào UI cũng sẽ không có quá nhiều tác hại.

```cpp
class DepositUI
{
 public:
 virtual void RequestDepositAmount() = 0;
};
class DepositTransaction : public Transaction
{
    public:
        DepositTransaction(DepositUI& ui)
        : itsDepositUI(ui)
        {}
        virtual void Execute()
        {
            ...
            itsDepositUI.RequestDepositAmount();
            ...
        }
    private:
        DepositUI& itsDepositUI;
};
class WithdrawalUI
{
    public:
        virtual void RequestWithdrawalAmount() = 0;
};
class WithdrawalTransaction : public Transaction
{
    public:
        WithdrawalTransaction(WithdrawalUI& ui)
        : itsWithdrawalUI(ui){}
        virtual void Execute()
        {
            ...
            itsWithdrawalUI.RequestWithdrawalAmount();
            ...
        }
    private:
        WithdrawalUI& itsWithdrawalUI;
};

class TransferUI
{
    public:
        virtual void RequestTransferAmount() = 0;
};

class TransferTransaction : public Transaction
{
 public:
    TransferTransaction(TransferUI& ui)
    : itsTransferUI(ui){}
    virtual void Execute()
    {
        ...
        itsTransferUI.RequestTransferAmount();
        ...
    }
 private:
    TransferUI& itsTransferUI;
};

class UI : public DepositUI
 , public WithdrawalUI
 , public TransferUI
{
    public:
        virtual void RequestDepositAmount();
        virtual void RequestWithdrawalAmount();
        virtual void RequestTransferAmount();
};
```
*Snippet 12-6*

Phân tích kỹ lưỡng *snippet 12-6*, ta nhận thấy một vấn đề của việc tuân thủ ISP mà ta không nhìn thấy rõ ràng trong ví dụ về `TimedDoor`. Chú ý rằng mỗi loại giao dịch cần phải biết về UI cần dùng tương ứng của chúng theo một cách nào đó. `DepositTransaction` phải biết về `DepositUI`, `WithdrawTransaction` phải biết về `WithdrawUI`, ... Trong *snippet 12-6*, ta xử lý vấn đề này bằng cách buộc mỗi giao dịch phải được cung cấp một UI tương ứng khi khởi tạo. Bằng cách này, ta có thể áp dụng thành ngữ khởi tạo như trong *snippet 12-7*.

```cpp
UI Gui; // global object;
void f()
{
    DepositTransaction dt(Gui);
}
```
*Snippet 12-7*

Việc này khá tiện, nhưng nó cũng đồng thời bắt mỗi giao dịch phải giữ một con trỏ trỏ đến UI tương ứng của nó. Một cách khác để xử lý vấn đề là tạo một bộ các biến toàn cục như *snippet 12-8*

```cpp
// in some module that gets linked in
// to the rest of the app.
static UI Lui; // non-global object;
DepositUI& GdepositUI = Lui;
WithdrawalUI& GwithdrawalUI = Lui;
TransferUI& GtransferUI = Lui;
// In the depositTransaction.h module
class WithdrawalTransaction : public Transaction
{
    public:
        virtual void Execute()
        {
            ...
            GwithdrawalUI.RequestWithdrawalAmount();
            ...
        }
};

```
*Snippet 12-8*

Trong *C++*, việc bỏ tất cả các biến toàn cục tương tự như ở *snippet 12-8* và trong cùng một class để tránh gây ô nhiễm namespace nghe có vẻ hấp dẫn (Xem *Snippet 12-9*). Tuy vậy, việc này lại có một tác dụng phụ không mong muốn. Để sử dụng `UIGlobals`, ta sẽ cần phải `#include "ui_globals.h"`, mà module này lại có `#include "depositUI.h"`, `#include "withdrawUI.h"`, và `#include "transferUI.h"`. Điều này nghĩa là bây giờ bất cứ module muốn dùng một UI nào đó sẽ, theo tính chất bắc cầu, phụ thuộc vào tất cả các UI khác nhau - đây chính là vấn đề mà nãy giờ ta muốn tránh. Bây giờ nếu bất cứ một module UI nào bị thay đổi, tất cả các module có `#include "ui_globals.h"` sẽ bị buộc phải compile lại. `UIGlobals` bây giờ lại hợp nhất tất cả các interface mà ta phải tốn bao công để phân tách!

```cpp
// in ui_globals.h
#include "depositUI.h"
#include "withdrawalUI.h"
#include "transferUI.h"
class UIGlobals
{
    public:
        static WithdrawalUI& withdrawal;
        static DepositUI& deposit;
        static TransferUI& transfer
};
// in ui_globals.cc
static UI Lui; // non-global object;
DepositUI& UIGlobals::deposit = Lui;
WithdrawalUI& UIGlobals::withdrawal = Lui;
TransferUI& UIGlobals::transfer = Lui;
```
*Snippet 12-9*

#### Monad và Polyad

Xét hàm `g` cần cả `DepositUI` và `TransferUI`, và ta muốn các UI này được đưa vào hàm dưới dạng biến. Vậy ta nên biểu diễn hàm này thế nào?

```cpp
void g(DepositUI&, TransferUI&); // polyadic form
```

Hay là

```cpp
void g(UI&); // monadic form
```

Việc áp dụng cách thứ 2 khá hấp dẫn khi mà với cách đầu ta biết rằng cả hai biến con trỏ sẽ cùng trở đến một đối tượng. Hơn thế nữa, nếu ta dùng cách đầu (polyadic), khi muốn gọi hàm *g*, ta sẽ phải gọi như thế này:

```cpp
g(ui,ui);
```

Trông nó khá kỳ cục nhỉ...

Bất kể có kỳ cục hay không, trong nhiều trường hợp, cách định nghĩa hàm thứ nhất vẫn nên được ưu tiên hơn. Đầu tiên, việc dùng kiểu monadic buộc `g` phải phụ thuộc vào tất cả các interface ở trong UI. Vậy là trong trường hợp `WithdrawUI` cần thay đổi, `g` cũng sẽ bị ảnh hưởng dù nó chẳng liên quan gì. Điều này còn tệ hơn `g(ui,ui)`! Hơn thế nữa, ta cũng không chắc rằng trong mọi hoàn cảnh 2 biến của `g` sẽ luôn trỏ vào cùng một đối tượng. Việc tất cả các interface này được hợp nhất trong cùng một vật thể là việc mà `g` không cần phải biết. Vì thế. tác giả ưu tiên cách gọi thứ nhất (polyadic) hơn.

**Nhóm các client.** - Các client của một lớp thường có thể được nhóm dựa vào các hàm mà nó cần gọi. Bằng cách này, một interface được tạo ra có thể phục vụ cho một nhóm các client và làm giảm số lượng interface mà lớp cần phải cụ thể hóa. Nó cũng làm cho lớp dịch vụ ở dưới không phải phụ thuộc vào mỗi client.

Đôi khi, một vài phương thức được sử dụng bởi 2 nhóm client khác nhau sẽ có những điểm chung. Nếu lượng điểm chung là nhỏ, những nhóm này vẫn nên được tách biệt ra. Những hàm dùng chung lúc này nên được khai báo ở interface cho cả 2 nhóm, và sau đó lớp server cụ thể sẽ kế thừa cả 2 interface nhưng chỉ hiện thực hóa các hàm bị trùng một lần.  

**Các interface hay thay đổi** - Khi các ứng dụng hướng đối tượng được cập nhật, các interface hiện hữu của các lớp và component cũng có thể phải thay đổi. Có những lúc mà những thay đổi này dẫn đến việc cả phần mềm của chúng ta bị ảnh hưởng vô cùng lớn và buộc ta phải deploy lại toàn bộ hệ thống. Một cách để giảm nhẹ nỗi đau này là thêm một interface mới thay vì việc chỉnh sửa interface hiện hữu. Các client của interface cũ nếu muốn chuyển qua dùng interface mới có thể chủ động hỏi đối tượng được truyền vào về interface mới này (*Snippet 12-10*).

```cpp
void Client(Service* s)
{
 if (NewService* ns = dynamic_cast<NewService*>(s))
 {
    // use the new service interface
 }
}

```
*Snippet 12-10*

Một lần nữa, ta cũng cần chú ý không lạm dụng việc này. Chỉ việc tưởng tượng một lớp xử lý hàng trăm interface, một vài cái cho version, một vài cho các nhóm client là đã đủ đáng sợ rồi. 
### Kết luận

Các lớp "fat" tạo ra những sự ràng buộc kỳ cục và tai hại giữa các clients của chúng. Khi một client của một lớp "fat" buộc lớp này phải thay đổi, tất cả các client khác của nó sẽ bị ảnh hưởng. Vì vậy, mỗi client chỉ nên phụ thuộc vào các methods mà họ thực sự gọi. Điều này có thể đạt được bằng cách chia nhỏ interface của lớp "fat" ra thành nhiều interface riêng biệt phục vụ 
trực tiếp cho mỗi client hoặc một nhóm client mà mỗi interface này sẽ chỉ khai báo những hàm mà client hay nhóm client này cần sử dụng. Lớp "fat" sau đó có thể kế thừa toàn bộ các interface này và triển khai nó. Điều này phá vỡ sự phụ thuộc của các clients lên những hàm mà họ khồng cần đến và cho phép các nhóm client này độc lập với nhau.




