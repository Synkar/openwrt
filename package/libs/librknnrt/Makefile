#
# Copyright (C) 2024 Synkar
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=librknnrt
PKG_VERSION:=2.3.2
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/librknnrt
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Rockchip RKNN runtime (librknnrt.so v2.3.2)
  DEPENDS:=@TARGET_rockchip +libstdcpp
endef

define Package/librknnrt/description
  Rockchip RKNN userspace runtime library (librknnrt.so) for the RK3588 NPU.
  Required by RKNN model inference: the closed vendor .so talks to the kernel
  rknpu driver (or the rkopnu out-of-tree module that presents the same ioctl
  ABI) via DRM render-node ioctls. Version 2.3.2 (build 429f97ae6b@2025-04-09),
  validated against the rkopnu driver. Sourced from the Rockchip rknpu2 SDK
  (external/rknpu2/runtime/Linux/librknn_api/aarch64).

  NOTE: this is a glibc-linked binary (NEEDED: libc.so.6, libpthread.so.0,
  libdl.so.2, libm.so.6, libstdc++.so.6, libgcc_s.so.1). On the default musl
  target it will NOT load without a glibc compatibility layer — install the
  gcompat package (not yet in the Synkar feeds) or switch the target libc to
  glibc (CONFIG_LIBC_USE_GLIBC) as a fallback.
endef

define Build/Compile
endef

define Package/librknnrt/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) ./files/librknnrt.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) ./files/rknn_api.h $(1)/usr/include/
endef

$(eval $(call BuildPackage,librknnrt))
