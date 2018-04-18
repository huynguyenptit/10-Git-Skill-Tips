#10 Lời khuyên để nâng kĩ năng về Git lên tầm cao mới

Gần đây, chúng tôi vừa đưa ra một vào hướng dẫn giúp bạn làm quen vs Git cơ bản và sử dụng Git trong môi trường làm việc nhóm. Các lệnh mà chúng tôi đã bàn luận đã đủ để giúp một developer tồn tại trong thế giới của Git. Trong bài viết này, chúng tôi sẽ cố nghiên cứu xem làm cách nào để thời gian của bạn trở nên hiệu quả và tận dụng các tính năng mà Git cung cấp.

Lưu ý: Một vài câu lệnh trong bài viết bao gồm một phần của câu lệnh nằm trong dấu ngoặc vuông (ví dụ git add -p [filename]). Trong những ví dụ đó, bạn có thể chèn số, các định danh, v.v. mà không cần dấu ngoặc vuông.


##1. Tự động hoàn thành lệnh Git

Nếu bạn chạy các lệnh của Git thông qua "command line", nó thật mệt mỏi khi mà cứ phải nhập các câu lệnh bằng tay mỗi lần dùng. Để cải thiện việc này, bạn có thể bật tự động hoành thành của các câu lệnh Git trong vòng vài phút.

Để có được script này, chạy lệnh sau trong một hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp theo, thêm dòng lệnh dưới đây vào trong file ```~/.bash_profile```

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập điều này trước đó, tôi vẫn không thể nhấn mạnh đủ: Nếu bạn muốn dùng tất cả các tính năng của Git, bạn nên chỉ định đổi chỗ cho giao diện command line!

##2. Loại bỏ File trong Git

Bạn có cảm thấy mệt mỏi khi biên dịch các file (như ```.pyc```) xuất hiện trong kho Git của bạn? Hoặc bạn có chán đến mức khi bạn thêm chúng vào Git? Nhìn xa hơn, có một cách để bạn có thể bảo Git bỏ qua một số tập tin và thư mục hoàn toàn. Đơn giản là tạo một file tên là ```.gitignore``` và liệt kê các file hoặc thư mục bạn không muốn Git theo dõi. Bạn có thể tạo ra các ngoại lệ sử dụng dấu !.

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3. Ai đã làm bẩn code của tôi?


Đó là bản năng tự nhiên của con người để đổ lỗi cho người khác khi có điều gì đó không ổn. Nếu server của bạn bị hỏng, rất dễ để tìm ra thủ phạm - chỉ cần ```git blame```. Câu lệnh này sẽ hiện ra tất cả người viết các dòng code trong file, commit mà thấy sự thay đổi cuối cùng trong dòng đó, và thời gian của commit.

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

Và trong ảnh chụp màn hình bên dưới, bạn có thể thấy lệnh này sẽ trông như thế nào trên một repository lớn hơn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Xem lại lịch sử của Repository


Chúng tôi đã xem xét việc sử dụng ```git log``` trong một hướng dẫn trước đó, tuy nhiên, có ba lựa chọn mà bạn nên biết.

* ```--oneline``` - Nén thông tin được hiển thị bên cạnh mỗi commit đến một commit đã được giảm nhẹ và các thông báo commit, tất cả được hiển thị trên cùng một dòng
* ```--graph``` – Tùy chọn này đưa ra một biểu diễn đồ họa dựa vào các đoạn văn bản lịch sử nằm bên phía tay trái đầu ra của các bạn. Không sử dụng nếu bạn đang xem lịch sử của một nhánh đơn.
* ```--all``` – Hiển thị tất cả lịch sử của các nhánh.

Dưới đây là sự kết hợp giữa các tùy chọn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. Đừng bao giờ để mất kiểm soát một Commit

Hãy nói rằng bạn đã commit một thứ gì đó bạn không hề muốn và kết thúc bằng việc thiết lập lại những điều đó để trở về bước trước. Sau đó, bạn nhận ra bạn đã mất đi một số thông tin trong quá trình và bạn muốn lấy lại nó, hoặc ít nhất xem lại chúng. Đó là điều mà ```git reflog``` có thể giúp.

Một câu lệnh ```git log``` đơn giản hiển thị commit cuối cùng, cha của chúng, hay cha của cha v.v. Tuy nhiên, ```git reflog``` lại là một danh sách các commit phần đầu đã được chỉ đến. Nhớ rằng nó nằm trong hệ thống của bạn; nó không phải là một phần repository của bạn và cũng không được bao gồm trong danh sách push hoặc merge.

Nếu tôi chạy ```git log```, tôi sẽ lấy ra những commit mà chúng là một phần repository của tôi:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, một câu lệnh ```git reflog``` hiển thị một commit (```b1b0ee9 – HEAD@{4}```) đã bị mất khi thôi thực hiện một thiết lập hard reset:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Phân loại các thành phần của một file đã thay đổi cho một Commit

Nói chung, một cách thực hành tốt là tạo ra các commit theo tính năng, nghĩa là, mỗi commit phải đại cho cho 1 tính năng hoặc sửa lỗi. Giả sử điều gì sẽ xảy ra nếu bạn sửa 2 lỗi hoặc thêm nhiều tính năng mà không commit các thay đổi. Trong tinh huống này, bạn có thể đưa những thay đổi vào trong một commit. Nhưng có một cách tốt hơn: Xếp các tập tin riêng lẻ và commit chúng riêng biệt.

Hãy nói rằng bạn đã tạo ra rất nhiều thay đổi đến một file và muốn cúng xuất hiện trong các commit riêng biệt. Trong trường hợp này, chúng ta sẽ thêm những file đó bằng tiền tố ```-p``` vào trong câu lệnh add.

```

git add -p [file_name]

```

Hãy cùng chứng minh điều tương tự Tôi đã thêm 2 dòng vào một ```file_name``` và tôi chỉ muốn cái dòng đầu tiên hoặc dòng thứ 3 xuất hiện trong commit của tôi. Hãy cùng xem ```git diff``` sẽ cho ta xem những gì.


![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi ta thêm tiền tố ```-p``` cho câu lệnh ```add```.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Có vẻ Git đã cho rằng tất cả những thay đổi đều là cùng thành phần của một ý tưởng(tính năng), do đó nó đã gom chúng thành một khối lớn. Bạn có những lựa chọn dưới đây:

* Nhập y để xếp lại khối.
* Nhập n để bỏ xếp khối
* Nhập e để chỉnh khối bằng tay
* Nhập d để thoát hoặc đến file tiếp theo.
* Nhập s để tách lẻ khối.

Trong trường hợp của chúng tôi, tôi thực sự muốn chia chúng thành các phần nhỏ để dễ chọn add vài file hoặc loại bỏ chúng.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Nhưng bạn có thể thấy, chúng tôi đã thêm vào dòng 1 và dòng 3 và đã bỏ qua dòng 2. Bạn có thể xem trạng thái của repository và tạo ra một commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Squash nhiều commit

Khi bạn gửi đi code để xem lại và tạo 1 pull request (Điều xảy ra thường xuyên trong một dự án mã nguồn mở), bạn có thể được hỏi cách để tạo ra một thay đổi trong code của bạn trước khi nó được chấp nhận. Bạn thực hiện thay đổi, chỉ để được yêu cầu thay đổi chúng trong những lần xem tiếp theo. Trước khi bạn biết nó, bạn có thêm vài commit. Lý tưởng nhất, bạn có thể sử dụng lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```
Nếu bạn muốn squash 2 commit cuối, câu lệnh này sẽ được chạy như dưới đây.

```

git rebase -i HEAD~2

```

Trong câu lệnh đang chạy, bạn sẽ được đưa đến một giao diện tương tác liệt kê các commit và hỏi lại xem cái nào bạn muốn squash. Lý tưởng nhất, bạn ```pick``` commit cuối cùng và ```squash``` cái đó.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn được yêu cầu cung cấp một thông báo về commit mới. Quá trình này chủ yếu viết lại lịch sử commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Stash lại những thay đổi không được commit

Hãy nói vs tôi rằng bạn đang làm sửa một lỗi nhất định hay làm một tính năng, và bạn đột nhiên được yêu cầu đưa ra công việc của bạn. Việc hiện tại của bạn chưa xong để được commit, và bạn không thể đưa ra trong giai đoạn này (mà không revert những thay đổi). Trong trường hợp này, ```git stash``` sẽ giải cứu bạn. Stash chủ yếu đưa tất cả những thay đổi của bạn và lưu trữ chúng cho trường hợp sử dụng sau này. Để stash những thay đổi của bạn, đơn giản bạn hãy chạy câu lệnh dưới đây.

```

git stash

```

Để kiểm tra danh sách các stash, bạn hãy chạy câu lệnh sau:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn bỏ stash và lấy lại những thay đổi chưa được commit, chạy câu lệnh sau:

```

git stash apply

```

Trong bức ảnh dưới đây, bạn có thể xem mỗi stash có một cái định danh, một số duy nhất (mặc dù bạn có mỗi một stash trong trường hợp này). Trong trường hợp bạn muốn apply các stash đã được chon, bạn hãy thêm định danh của chúng vào câu lệnh ```apply```:

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)

##9. Kiểm tra các commit đã thất lạc

Mặc dù ```reflog``` là một cách để kiểm tra các commit thất lạc, điều này là không khả thi đối vs các repository lớn. Trong khi câu lệnh ```fsck``` (Hệ thống kiếm tra file) có vẻ ổn.

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Dưới đây bạn có thể xem các commit đã thất lạc. Bạn có thể kiểm tra những thay đổi của các commit bằng cách chạy lệnh ```git show [commit_hash]``` hoặc xem chúng bằng cách chạy ```git merge [commit_hash]```.

```git fsck``` có một lợi thế so với ```reflog```. Hãy nói rằng bạn đã xóa một remote branch và sau đó clone repository. Với ```fsck``` bạn có thể tìm kiếm và xem lại những remote branch đã xóa.

##10. Cherry Pick (Vặt dâu)

Tôi đã nhớ hầu hết các lệnh của Git cho đến tận giờ. Và lệnh ```cherry-pick``` là lệnh tôi yêu thích nhất, vì nghĩa đen cũng như những tiện ích của nó!

Trong giới hạn đơn giản nhất, ```cherry-pick``` đang chọn một commit từ một nhánh khác và merge chúng với nhánh hiện tại. Nếu bạn làm đang làm việc theo kiểu song song 2 nhánh hoặc nhiều hơn, bạn có thể nhận thấy một lỗi xảy ra trên các nhánh. Nếu bạn giải quyết chúng trong một nhánh, bạn có thể cherry pick commit đó vào một nhánh khác mà không cần động tới các file hoặc commit khác

Hãy xem xét kịch bản chúng ta có thể sử dụng chúng. Tôi có 2 nhánh và tôi muốn ```cherry-pick``` commit ```b20fd14: Cleaned junk``` vào một nhánh khác.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi chuyển vào nhánh tôi muốn ```cherry-pick``` commit, và chạy câu lệnh sau:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù chúng tôi đã có một clean ```cherry-pick``` trong trường hợp này, bạn nên biết câu lệnh này có thể dẫn tới xung đột, vì vậy hãy cẩn trọng khi sử dụng

##Kết luận

Với điều này, chúng ta sẽ đến phần kết của danh sách các lời khuyên mà tôi nghĩ sẽ giúp cải thiện kĩ năng về Git cho bạn. Git là một công cụ tốt nhất hiện có và có thể làm được bất kể thứ gì bạn tưởng tượng. Do đó hãy cố thử thách bản thân vs Git. Rất có thể bạn sẽ học được rất nhiều thứ mới!!






