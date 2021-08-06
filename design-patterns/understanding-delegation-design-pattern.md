# Cách MÌNH hiểu về delegation design pattern
Tham khảo từ https://www.growingwiththeweb.com/2012/07/design-patterns-delegation-pattern.html

## Mục đích
Kiểu thiết kế này cho phép một object có thể ủy quyền việc thực thi một hay nhiều task cho một object khác (helper object). 
Hơi trừu tượng, vì người viết cũng chưa thể làm rõ hơn, từ từ hiểu rõ sẽ giải thích kỹ hơn.

Hiểu kiểu như 1 object định nghĩa ra một số công việc cần hoàn thành (định nghĩa ra tên function), còn việc hoàn thành nó như thế nào là việc của helper class (định nghĩa thân function).
Chỗ này lưu ý là đừng có hiểu nhầm 2 cái đang nói tới là một cái interface (định nghĩa hàm), cái còn lại là cái class implement interface đó. KHÔNG PHẢI VẬY ĐÂU.

Phần chính của kiểu thiết này gồm có 2 đối tượng chính.
### Delegate
### Delegator
