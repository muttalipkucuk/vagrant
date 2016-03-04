---
layout: "docs"
page_title: "Chef Apply - Provisioning"
sidebar_current: "provisioning-chefapply"
description: |-
  The Vagrant Chef Apply provisioner allows you to provision the guest using
  Chef with chef-apply.
---

# Chef Apply Provisioner

**Provisioner name: `chef_apply`**

The Vagrant Chef Apply provisioner allows you to provision the guest using
[Chef](https://www.getchef.com/), specifically with
[Chef Apply](https://docs.getchef.com/ctl_chef_apply.html).

Chef Apply is ideal for people who are already experienced with Chef and the
Chef ecosystem. Specifically, this documentation page does not cover how use
Chef or how to write Chef recipes.

<div class="alert alert-warning">
  <strong>Warning:</strong> If you are not familiar with Chef and Vagrant already,
  we recommend starting with the <a href="/docs/provisioning/shell.html">shell
  provisioner</a>.
</div>

## Options

This section lists the complete set of available options for the Chef Apply
provisioner. More detailed examples of how to use the provisioner are
available below this section.

* `recipe` (string) - The raw recipe contents to execute using Chef Apply on
  the guest.

* `log_level` (string) - The log level to use while executing `chef-apply`. The
  default value is "info".

* `upload_path` (string) - **Advanced!** The location on the guest where the
  generated recipe file should be stored. For most use cases, it is unlikely you
  will need to customize this value. The default value is
  `/tmp/vagrant-chef-apply-#` where `#` is a unique counter generated by
  Vagrant to prevent collisions.

In addition to all the options listed above, the Chef Apply provisioner supports
the [common options for all Chef provisioners](/docs/provisioning/chef_common.html).

## Specifying a Recipe

The easiest way to get started with the Chef Apply provisioner is to just
specify an inline
[Chef recipe](https://docs.chef.io/recipes.html). For
example:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision "chef_apply" do |chef|
    chef.recipe = "package[apache2]"
  end
end
```

This causes Vagrant to run Chef Apply with the given recipe contents. If you are
familiar with Chef, you know this will install the apache2 package from the
system package provider.

Since single-line Chef recipes are rare, you can also specify the recipe using a
"heredoc":

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision "chef_apply" do |chef|
    chef.recipe = <<-RECIPE
      package "apache2"

      template "/etc/apache2/my.config" do
        # ...
      end
    RECIPE
  end
end
```

Finally, if you would prefer to store the recipe as plain-text, you can set the
recipe to the contents of a file:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision "chef_apply" do |chef|
    chef.recipe = File.read("/path/to/my/recipe.rb")
  end
end
```

## Roles

The Vagrant Chef Apply provisioner does not support roles. Please use a
different Vagrant Chef provisioner if you need support for roles.

## Data Bags

The Vagrant Chef Apply provisioner does not support data_bags. Please use a
different Vagrant Chef provisioner if you need support for data_bags.