# devops-netology
Home work
НОВАЯ СТРОКА
В файле README.md опишите своими словами какие файлы будут проигнорированы в будущем благодаря добавленному .gitignore.

1. **/.terraform/* 
вложенный каталог /.terraform/ и всё его содержимое
2. *.tfstate
файлы с расширением tfstate
3. *.tfstate.*
файлы, содержащие в имени последовательность .tfstate.
4. crash.log
файлы crash.log - логи "падений"
5. *.tfvars
файлы с расширением tfvars, которые могут содержать приватную информацию :пароли, ключи и т.п.
6. override.tf
override.tf.json
*_override.tf
*_override.tf.json
Файлы, которые переопределяют ресурсы локально 
7. .terraformrc
terraform.rc
Файлы с расширением .terraformrc  и файлы terraform.rc - конфигурация командной строки