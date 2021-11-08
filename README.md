# devops-netology
first line
second line

# Исключить все файлы в директориях .terraform, находящихся во всех каталогах с любой вложенностью.
**/.terraform/*

# Исключить все файлы с расширением .tfstate
# шаблон также может совпадать на любом уровне ниже уровня .tfstate.
*.tfstate
*.tfstate.*

# Исключить файлы crash.log
crash.log

# Исключить все папки .tfvars
*.tfvars

# Исключить файлы override.tf
# Исключить файлы override.tf.json
# Исключить файлы с любой комбинацией в имени, заканчивающейся на _override.tf
# Исключить файлы с любой комбинацией в имени, заканчивающейся на _override.tf.json
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Исключить скрытую папку .terraformrc
# Исключить файлы terraform.rc
.terraformrc
terraform.rc

