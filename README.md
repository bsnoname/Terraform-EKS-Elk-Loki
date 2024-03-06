Nén 5 file sau thành 1 file alepay-aws.part1.rar, alepay-aws.part2.rar, alepay-aws.part3.rar,alepay-aws.part4.rar,alepay-aws.part5.rar.
* Đầu tiên trên hệ thống aws cần tạo ra 1 cụm EKS.
  Xem video này để kết nối terraform với aws : https://www.youtube.com/watch?v=iMqeu9XDTxI
  1. Để chạy đc terraform, cần tạo file main.tf "file này chứa tất cả các module cần triển khai cho hệ thống điển hình như EKS", variable.tf "File này là file chứa các biến của file main"
  2. Sau khi chạy được terraform, cần xem các module cần triển khai, như EKS, monitor
  3. File state sẽ được lưu ở trên S3.

Flow như sau:
1. Gitlab "đánh tag"
2. Chạy Pipelines
3. Git-runner "runner-all-group-8.198" sẽ thực hiện việc chạy CI/CD
   .gitlab-ci.yml "File này khai báo trên hệ thống" chạy Ci/CD cho hệ thống.

Flow Deploy app lên argocd.
Phần này sẽ được tích hợp trong file nén argocd-alepay-aws-release.tar.gz, đẩy file sau lên gitlab.
1. Argocd cấu hình tới file gitlab chứa source deploy.
   kustomization.yaml "Trong thư mục trên file này chứa thông tin images như tên của images"
   alepay-bi-report.yaml "File này chứa các thông tin cấu hình app như biến, mount ổ, ..."
2. Gitlab của dự án sẽ được deploy bằng cách sửa images trên file kustomization.yaml
3. Sau khi sửa xong images, argocd sẽ thấy file có sự thay đổi và sẽ thực hiện deploy app mới với images vừa được sửa xong trên file kustomization.yaml
4. Quá trình deploy được thực hiện một cách nhanh gọn
