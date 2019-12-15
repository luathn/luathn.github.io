---
title: "Chương trình Python đầu tiên: Tìm và nghe nhạc online trên terminal"
categories:
  - python
tags:
  - linux
  - python
---
## Ý tưởng
Để nói về ý tưởng của ứng dụng này thì phải kể đến cơ duyên với những ứng dụng chạy trên Terminal. Đầu tiên là Vim - một text editor rất hay ho, món này thì mình cũng đã biết từ khi còn là sinh viên nhưng lại không có lí do gì để phải tìm hiểu cả đến khi đi làm thấy vài đàn anh trong nghề xài nhìn quá chi là pro nên cũng mày mò học đòi. Tiếp theo là rất nhiều tool như zsh tmux, ncmpcpp, mps-youtube, ranger,... Tất cả những thứ trên là cảm hứng để thực hiện project này.

Khi đang lò mò mở trình duyệt để nghe nhạc vào mỗi sáng như mọi khi thì trong đầu chợt nghĩ có tool nào nghe nhạc online trên Terminal không nhỉ? Thế là nghía xem đủ ngóc ngách của google nhưng lại không có cái nào đúng ý cả. Thôi hay là viết cái tool nho nhỏ để mình xài có vẻ hay nè. Chiến thôi. Mà viết bằng gì nhỉ? Ruby có ổn hay không? Lại một hồi lục google thì tự thấy Python là ổn hơn cả.

## Tại sao là Python
* Python giờ là ngôn ngữ phổ biến nhất thể giới (theo thống kê [ở đây](https://youtu.be/Og847HVwRSI))
* Python rất mạnh mẽ nhờ bộ thư viện chuẩn cũng như thư viện ngoài có thể cài thêm, làm được hầu hết những gì mình cần.
* Rất nhiều ứng dụng cho Linux hiện nay được viết bằng Python cho thấy nó rất phù hợp với ý tưởng này.
* Triết lý cũng Python rất hay nên làm cho mình khá hứng thú để học ([Wikipedia](https://vi.wikipedia.org/wiki/Python_(ng%C3%B4n_ng%E1%BB%AF_l%E1%BA%ADp_tr%C3%ACnh)))

## Thực hiện thôi
Ý tưởng đã có rồi, công cụ cũng xịn luôn rồi, giờ thực hiện thôi nào:
### Tổng quát
Được định hình ban đầu là ứng dụng chạy trên terminal và nhỏ gọn nhất cả thể nên nó chỉ gồm 2 tính năng cơ bản và đầy đủ nhất đó là:
1. Tìm kiếm bài hát.
2. Phát bài hát đó.
Sau khi tìm thử về api để làm 2 điều trên thì gần như là bất khả thi nên mình quyết định chọn chiasenhac.com, trang này dữ liệu dễ lấy được nhất.

Ở lý do chọn Python mình có nói là rất hứng thú với Python vì triết lý của nó, với Linux cũng tưởng tự, Linux có những triết lí thiết kế nó rất hay mà một trong số đó là:
> Do One Thing and Do It Well

Và theo triết lý đó cho nên ứng dụng này cũng chia nhỏ ra, và phần đầu tiên là lấy được link download bài hát từ link bài hát.
### Get link download
```
import re
import requests

def get_download_link(song_url, quality=128):
    quality_dict = {128: 0, 320: 1, 500: 2}
    download_urls = []

    try:
        response = requests.get(song_url)
        html = response.content.decode("utf-8")
        pattern = r"download_item\" href=\"(.*?)\" title"
        for url in re.findall(pattern, html):
            download_urls.append(url)
        return download_urls[quality_dict[quality]]
    except:
        return
```
Không cần giải thích quá nhiều về đoạn code trên. Đó là 1 method với đầu vào là link bài hát và chất lượng mong muốn, đầu ra sẽ là list các link để download với chất lượng khác nhau.
Vì việc tìm cách lấy được và sử dụng các API của những trang nghe nhạc này là khá khó nên mình chọn cách giải quyết nhanh hơn đó là gửi request đến link cần rồi lấy hết html về. Sau khi có được đống html thì dùng regex để tách được link download mà mình cần, EZ.
### Nghe nhạc và Chill thôi
Đã lấy được link download trên rồi thì phần dưới này là tìm nhạc và phát nhạc thôi. À mà có link direct download rồi thì giờ làm sao phát nhạc đây? Như đã nói thì triết lí của Linux rất là hay, có rất nhiều tool có thể dùng để phát được cái link này và mình dùng [MPV](https://github.com/mpv-player/mpv)

Phần code này khá dài nên sẽ không đưa lên đây, sẽ có một ít screenshot để xem nó hoạt động ra sao.
Ứng dụng mở lên với hướng dẫn cực kì đơn giản đó là gõ /tên bài hát để tìm và ấn Ctrl-C để thoát
![Music in Terminal](https://images.viblo.asia/ed56fa68-f2a3-42b3-b1e9-9d2b6faaee59.png)

Sau khi gõ /tên bài và enter thì kết quả sẽ trả về dưới dạng list để mình có thể lựa chọn để nghe
![](https://images.viblo.asia/1d3b0380-58a4-4087-8c06-f7dbb3f4e582.png)

Giờ thì nhập số tương ứng để nghe và Chill nào:
![](https://images.viblo.asia/5b4dd129-8fb5-4408-99f5-4da5612bc46a.png)

## Tổng kết
Với một chương trình Python nhỏ nhắn và tiện lợi như trên thì cũng đủ để học hỏi được khá nhiều điều từ ngôn ngữ này. Có những kiến thức không hẳn về kĩ thuật mà là về triết lí thiết kế nên nó rất hay mà cần phải tìm hiểu nhiều hơn nữa để có cái nhìn sâu hơn.
Ứng dụng này bây giờ mới chỉ dừng lại ở mức xài được chứ chưa đầy đủ những tính năng cần thiết, phần này mình sẽ viết thêm trong tương lai (TODO).
