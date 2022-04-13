# devops-netology  
Home work 05-virt-04-docker-compose

Домашнее задание к занятию "5.4. Оркестрация группой Docker контейнеров на примере Docker Compose"

Задача 1
Создать собственный образ операционной системы с помощью Packer.

Для получения зачета, вам необходимо предоставить:

Скриншот страницы, как на слайде из презентации (слайд 37).
````
Вот тут не совсем понимаю условие, т.к. 37й слайд немного не про эту задачу.

Я пробовал создать образ debian 11

{
  "builders": [
    {
      "type":      "yandex",
      "token":     "XXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "folder_id": "b1g3tauldlfvmch32u0h",
      "zone":      "ru-central1-b",

      "image_name":        "debian-11-{{isotime | clean_resource_name}}",
      "image_family":      "debian11",
      "image_description": "my custom debian 11",

      "source_image_family": "debian-11",
      "subnet_id":           "e2l35kqt3bvbugf9885r",
      "use_ipv4_nat":        true,
      "disk_type":           "network-ssd",
      "ssh_username":        "debian"
    }
  ],
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "echo 'Ready'"
      ]
    }
  ]
 }

 ./packer build 1.json
yandex: output will be in this color.

==> yandex: Creating temporary RSA SSH key for instance...
==> yandex: Using as source image: fd8iea4hktad0qe0nogj (name: "debian-11-v20220207", family: "debian-11")
==> yandex: Use provided subnet id e2l35kqt3bvbugf9885r
==> yandex: Creating disk...
==> yandex: Creating instance...
==> yandex: Waiting for instance with id epdhh78bp6atrtvatqt7 to become active...
    yandex: Detected instance IP: 51.250.101.232
==> yandex: Using SSH communicator to connect: 51.250.101.232
==> yandex: Waiting for SSH to become available...
==> yandex: Connected to SSH!
==> yandex: Provisioning with shell script: /tmp/packer-shell3080222920
    yandex: Ready
==> yandex: Stopping instance...
==> yandex: Deleting instance...
    yandex: Instance has been deleted!
==> yandex: Creating image: debian-11-2022-04-12t18-50-35z
==> yandex: Waiting for image to complete...
==> yandex: Success image create...
==> yandex: Destroying boot disk...
    yandex: Disk has been deleted!
Build 'yandex' finished after 1 minute 34 seconds.

==> Wait completed after 1 minute 34 seconds

==> Builds finished. The artifacts of successful builds are:
--> yandex: A disk image was created: debian-11-2022-04-12t18-50-35z (id: fd8u2asqasc0mam9lf84) with family name debian11


И у меня действительно получился в яндекс облаке соотв образ.



````


Задача 2
Создать вашу первую виртуальную машину в Яндекс.Облаке.

Для получения зачета, вам необходимо предоставить:

Скриншот страницы свойств созданной ВМ, как на примере ниже:

````
 ./terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of yandex-cloud/yandex...
- Installing yandex-cloud/yandex v0.73.0...
- Installed yandex-cloud/yandex v0.73.0 (self-signed, key ID E40F590B50BB8E40)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.



 ./terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # yandex_compute_instance.node01 will be created
  + resource "yandex_compute_instance" "node01" {
      + allow_stopping_for_update = true
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = "node01.netology.cloud"
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                centos:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDfZpG6zzGYcc+NMgD42PE8BOU4clSEjrPe8VQAOc3ApdrwsG1wGicoPuL9v0sLMjjaWY+qmNURbuuDVkky1p9T/jvXKP/On2oPvzdP96IlHjaoF4FfpR/koDTfCZ1mHjAIUw7G70LZ4Z2EzOGUPoBNN+PVL3/h7DnBaSmSH9Tg+ea73fLJ3UNIeWALVH9vvn4ln2QSodVgvWuOokY8k8SdR3ZBI5xV7oyDJhSfLQAix/E74VNEULpjZ0vs1SKiLSKUkHCDVmUfn7d4RK8Jf6Fp8DWZ0G+1DtN15tzph/mKJEezA3UjxceXpskjAvxUypLgaq0vF4CenZpeDJObPAYxUbnXLxlDxq8B35y2SxPB+85hY4/i6BnwFJSvoONsBlppj0jP18GXDNpMwOMr+VCqbHZmyhibc7zSEd+VIEyY8s1//89SxVNhIe19kAwedU0vROZobsdcoCsmW83FZc8FR9MltzNGk9X+f4e7Kk4qls+5qOW1KTIZ0RUv9MyNpcM= root@msk-dock01
            EOT
        }
      + name                      = "node01"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8u2asqasc0mam9lf84"
              + name        = "root-node01"
              + size        = 50
              + snapshot_id = (known after apply)
              + type        = "network-nvme"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 8
          + memory        = 8
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_vpc_network.default will be created
  + resource "yandex_vpc_network" "default" {
      + created_at                = (known after apply)
      + default_security_group_id = (known after apply)
      + folder_id                 = (known after apply)
      + id                        = (known after apply)
      + labels                    = (known after apply)
      + name                      = "net"
      + subnet_ids                = (known after apply)
    }

  # yandex_vpc_subnet.default will be created
  + resource "yandex_vpc_subnet" "default" {
      + created_at     = (known after apply)
      + folder_id      = (known after apply)
      + id             = (known after apply)
      + labels         = (known after apply)
      + name           = "subnet"
      + network_id     = (known after apply)
      + v4_cidr_blocks = [
          + "192.168.101.0/24",
        ]
      + v6_cidr_blocks = (known after apply)
      + zone           = "ru-central1-a"
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + external_ip_address_node01_yandex_cloud = (known after apply)
  + internal_ip_address_node01_yandex_cloud = (known after apply)

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.


 ./terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # yandex_compute_instance.node01 will be created
  + resource "yandex_compute_instance" "node01" {
      + allow_stopping_for_update = true
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = "node01.netology.cloud"
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                centos:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDfZpG6zzGYcc+NMgD42PE8BOU4clSEjrPe8VQAOc3ApdrwsG1wGicoPuL9v0sLMjjaWY+qmNURbuuDVkky1p9T/jvXKP/On2oPvzdP96IlHjaoF4FfpR/koDTfCZ1mHjAIUw7G70LZ4Z2EzOGUPoBNN+PVL3/h7DnBaSmSH9Tg+ea73fLJ3UNIeWALVH9vvn4ln2QSodVgvWuOokY8k8SdR3ZBI5xV7oyDJhSfLQAix/E74VNEULpjZ0vs1SKiLSKUkHCDVmUfn7d4RK8Jf6Fp8DWZ0G+1DtN15tzph/mKJEezA3UjxceXpskjAvxUypLgaq0vF4CenZpeDJObPAYxUbnXLxlDxq8B35y2SxPB+85hY4/i6BnwFJSvoONsBlppj0jP18GXDNpMwOMr+VCqbHZmyhibc7zSEd+VIEyY8s1//89SxVNhIe19kAwedU0vROZobsdcoCsmW83FZc8FR9MltzNGk9X+f4e7Kk4qls+5qOW1KTIZ0RUv9MyNpcM= root@msk-dock01
            EOT
        }
      + name                      = "node01"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8u2asqasc0mam9lf84"
              + name        = "root-node01"
              + size        = 50
              + snapshot_id = (known after apply)
              + type        = "network-nvme"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 8
          + memory        = 8
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_vpc_network.default will be created
  + resource "yandex_vpc_network" "default" {
      + created_at                = (known after apply)
      + default_security_group_id = (known after apply)
      + folder_id                 = (known after apply)
      + id                        = (known after apply)
      + labels                    = (known after apply)
      + name                      = "net"
      + subnet_ids                = (known after apply)
    }

  # yandex_vpc_subnet.default will be created
  + resource "yandex_vpc_subnet" "default" {
      + created_at     = (known after apply)
      + folder_id      = (known after apply)
      + id             = (known after apply)
      + labels         = (known after apply)
      + name           = "subnet"
      + network_id     = (known after apply)
      + v4_cidr_blocks = [
          + "192.168.101.0/24",
        ]
      + v6_cidr_blocks = (known after apply)
      + zone           = "ru-central1-a"
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + external_ip_address_node01_yandex_cloud = (known after apply)
  + internal_ip_address_node01_yandex_cloud = (known after apply)

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

yandex_vpc_network.default: Creating...
yandex_vpc_network.default: Still creating... [10s elapsed]
yandex_vpc_network.default: Creation complete after 18s [id=enpkjasp43huhbk8pa8k]
yandex_vpc_subnet.default: Creating...
yandex_vpc_subnet.default: Creation complete after 1s [id=e9boin8a4ushu1t7nmbr]
yandex_compute_instance.node01: Creating...
yandex_compute_instance.node01: Still creating... [10s elapsed]
yandex_compute_instance.node01: Still creating... [20s elapsed]
yandex_compute_instance.node01: Still creating... [30s elapsed]
yandex_compute_instance.node01: Creation complete after 35s [id=fhm2inop5c5u15nsi5ps]

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

Outputs:

external_ip_address_node01_yandex_cloud = "51.250.4.68"
internal_ip_address_node01_yandex_cloud = "192.168.101.27"



````


Задача 3
Создать ваш первый готовый к боевой эксплуатации компонент мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить:

Скриншот работающего веб-интерфейса Grafana с текущими метриками, как на примере ниже
