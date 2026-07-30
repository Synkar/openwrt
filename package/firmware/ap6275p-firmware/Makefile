#
# Copyright (C) 2024 Synkar
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ap6275p-firmware
PKG_VERSION:=1
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/ap6275p-firmware
  SECTION:=firmware
  CATEGORY:=Firmware
  TITLE:=Broadcom AP6275P firmware (BCM4375 WiFi + BCM4362 BT)
  DEPENDS:=@TARGET_rockchip
endef

define Package/ap6275p-firmware/description
  WiFi + Bluetooth firmware for the Ampak AP6275P combo module used on the
  RD-BOX-RK3588. WiFi is Broadcom BCM4375/BCM43752 (PCIe, brcmfmac) and
  Bluetooth is BCM4362A2 (UART). Blobs sourced from the Rockchip vendor SDK
  (external/rkwifibt/firmware/broadcom/AP6275_PCIE).
endef

define Build/Compile
endef

define Package/ap6275p-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/brcm
	# O chip reporta BCM43752, entao o brcmfmac pede brcmfmac43752-pcie.*.
	$(INSTALL_DATA) ./files/brcm/brcmfmac43752-pcie.bin $(1)/lib/firmware/brcm/
	$(INSTALL_DATA) ./files/brcm/brcmfmac43752-pcie.clm_blob $(1)/lib/firmware/brcm/
	$(INSTALL_DATA) ./files/brcm/brcmfmac43752-pcie.txt $(1)/lib/firmware/brcm/
	$(INSTALL_DATA) ./files/brcm/BCM4362A2.hcd $(1)/lib/firmware/brcm/
	# btbcm may look in /lib/firmware as well as /lib/firmware/brcm; ship both.
	$(INSTALL_DATA) ./files/BCM4362A2.hcd $(1)/lib/firmware/
endef

$(eval $(call BuildPackage,ap6275p-firmware))
