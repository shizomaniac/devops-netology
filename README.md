New branch - fix


# devops-netology
First edit

#Игнорируются все файлы в скрытых папках .terraform нахдящихся в любой подпапке папки terraform, которую мы создали 
**/.terraform/*  

# Все файлы с расширением .terraform и все, которые содержат в названии .terraform.
*.tfstate
*.tfstate.*

# Игнорируется файл с именем crash.log и все файлы с именами начинающимися с crash. и заканичивающимися на .log
crash.log
crash.*.log

 
# Все файлы с расширениями .tfvars и .tfvars.json
*.tfvars
*.tfvars.json

# Файлы override.tf и override.tf.json и все файлы имена которых заканчиваются на _override.tf и _override.tf.json
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Игнорируются файлы .terraformrc и terraform.rc 
.terraformrc
terraform.r
