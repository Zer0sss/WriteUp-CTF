#Do web không vào lại được nên không biết tên Challenges
![image](https://user-images.githubusercontent.com/88301536/139675875-c4820cf1-4b3f-4317-aa15-83c19a398f50.png)
Có thể thấy đây là một pcapng bắt gói tin theo Telnet nào đó
Follow TCP Stream để theo dõi, xem xét từng luồng
![image](https://user-images.githubusercontent.com/88301536/139676174-63e02c35-f409-45db-b41a-57c1e22bc7c3.png)
Có thể thấy client kết nối tới server với user: user22 với mật khẩu là "33a465747cb15e84a26564f57cda0988" và không làm gì hết cả
Nhận thấy mật khẩu này khả nghi thì đây chính là MD5
Đảo ngược hàm băm ta được flag
##FLAG: kqctf{dancingqueen}

