#
# Copyright (C) 2024 Synkar
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mali-csffw
PKG_VERSION:=1
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/mali-csffw
  SECTION:=firmware
  CATEGORY:=Firmware
  TITLE:=ARM Mali CSF firmware (Mali-G610 Valhall)
  DEPENDS:=@TARGET_rockchip
endef

define Package/mali-csffw/description
  Mali CSF firmware blob for the ARM Mali-G610 GPU on the RD-BOX-RK3588
  (rk3588). Required by the mainline panthor DRM driver, which loads it via
  request_firmware() as "arm/mali/arch10.8/mali_csffw.bin" (arch10.8 = Valhall
  G610). Sourced from the Rockchip vendor SDK (external/libmali/firmware/g610).
endef

define Build/Compile
endef

define Package/mali-csffw/install
	$(INSTALL_DIR) $(1)/lib/firmware/arm/mali/arch10.8
	$(INSTALL_DATA) ./files/arm/mali/arch10.8/mali_csffw.bin \
		$(1)/lib/firmware/arm/mali/arch10.8/
endef

$(eval $(call BuildPackage,mali-csffw))
