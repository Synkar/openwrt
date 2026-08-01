#
# Copyright (C) 2024 Synkar
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rkopnu
PKG_VERSION:=0.1
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0

VIDEO_MENU?=Video Support

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rkopnu
  SUBMENU:=$(VIDEO_MENU)
  TITLE:=Rockchip RK3588 NPU driver (vendor rknpu ioctl ABI)
  DEPENDS:=@TARGET_rockchip +kmod-drm +kmod-drm-sched +kmod-drm-shmem-helper
  FILES:=$(PKG_BUILD_DIR)/rkopnu.ko
  AUTOLOAD:=$(call AutoProbe,rkopnu)
  KCONFIG:=
endef

define KernelPackage/rkopnu/description
  Out-of-tree driver for the Rockchip RK3588 NPU that presents the vendor
  rknpu ioctl ABI on a mainline kernel, so the closed vendor userspace runtime
  librknnrt.so runs without the Rockchip BSP kernel. Forked from mainline
  drivers/accel/rocket (same DT compatible rockchip,rk3588-rknn-core); reuses
  rocket's GEM/shmem, drm_sched, IOMMU and threaded IRQ plumbing, adds the
  register-command/job-submit programming the vendor rknpu driver does.
  Requires the in-tree rocket driver NOT be loaded (CONFIG_DRM_ACCEL_ROCKET
  unset in the target config).
endef

define Build/Compile
	$(KERNEL_MAKE) M="$(PKG_BUILD_DIR)" modules
endef

$(eval $(call KernelPackage,rkopnu))
