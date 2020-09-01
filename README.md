# canvas/base-image-ova

This is a base image for canvas built on top off [canvas/base-image-iso](https://github.com/canvas-lms/base-image-iso) rom an ova image.
It adds canvas dependencies like node, ruby and postgres to the base ubuntu box.

# Usage
Prebuilt images (`*.ova` for virtualbox and `*.box` for vagrant) are available at 
[Github](https://github.com/canvas-lms/base-image-ova/releases/). See the Build section for instructions on
building locally.

## As a base image for packer
The image can be used as a base image for packer to build onto. 

```json
// Insert packer.json
```

## As a Vagrant box
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "../../canvas-org/base-image-iso/dist/packer-ubuntu64.box"
  config.ssh.username = "ubuntu"
  config.ssh.password = "ubuntu"
  
  # The box comes without the guest additions installed so default mounting with vboxfs would fail. 
  # You can install the guest additions your self, use the vagrant-vbguest plugin (https://github.com/dotless-de/vagrant-vbguest)
  # or choose rsync or smb sync.
  config.vm.synced_folder ".", disabled: true
end

```

## As a standalone virtualbox vm
The image can also be used to create a virtual machine without vagrant, just open the `.ova` file with virtualbox.

# Build
Unfortunatelly virtualbox images cannot be built on CI Services due to limitations on nested virtualization. 
So here are the steps needed to build the images locally.

```shell script
packer validate packer.json
packer build packer.json
```
