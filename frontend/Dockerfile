# 1. Configuration (We'll use the local provider to create a file)
terraform {
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "2.4.0"
    }
  }
}

# 2. Input Variables (The "Raw" data)
variable "app_name" {
  type    = string
  default = "   Super-App-2026   " # Notice the messy spaces
}

# 3. Locals (The "Clean-up" logic)
locals {
  # We trim the spaces and make it lowercase
  clean_name = trimspace(lower(var.app_name))
  
  # We define a standard set of metadata
  file_content = <<EOT
  Application Name: ${local.clean_name}
  Deployment Date:  ${formatdate("DD MMM YYYY", timestamp())}
  Status:           Active
  EOT
}

# 4. Resource (Using the locals to do work)
resource "local_file" "app_config" {
  content  = local.file_content
  filename = "${path.module}/config-${local.clean_name}.txt"
}

# 5. Output (To see the result in the terminal)
output "result" {
  value = "Success! Created file: config-${local.clean_name}.txt"
}
