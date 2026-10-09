#
# Copyright (C) 2026 Synkar
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=bcmdhd
PKG_VERSION:=101.10.591.52
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0

WMENU:=Wireless Drivers

include $(INCLUDE_DIR)/package.mk

# Backports headers (mac80211/cfg80211): the driver must be compiled against
# these, NOT the in-tree kernel headers, because ieee80211_channel and other
# cfg80211 structs differ (extra dfs_state_last_available member). The
# backports tree is built by package/kernel/mac80211 (kmod-cfg80211).
BACKPORTS_DIR:=$(patsubst %/,%,$(dir $(LINUX_DIR)))/mac80211-regular/backports-$(LINUX_VERSION)
BCMDHD_BACKPORTS_INCLUDE:=-I$(BACKPORTS_DIR)/backport-include/ \
	-I$(BACKPORTS_DIR)/backport-include/uapi \
	-I$(BACKPORTS_DIR)/include/ \
	-I$(BACKPORTS_DIR)/include/uapi \
	-include $(BACKPORTS_DIR)/backport-include/backport/backport.h

define KernelPackage/bcmdhd
  SUBMENU:=$(WMENU)
  TITLE:=Broadcom DHD vendor driver for AP6275P/BCM43752 (WiFi6/11ax)
  DEPENDS:=@TARGET_rockchip +kmod-cfg80211
  FILES:=$(PKG_BUILD_DIR)/bcmdhd.ko
  KCONFIG:=
endef

define KernelPackage/bcmdhd/description
  Out-of-tree Rockchip vendor bcmdhd driver (101.10.591.x, from the RK3588
  kernel-6.1 SDK), ported to kernel 6.18 for the Ampak AP6275P (BCM43752)
  PCIe module. This is the only driver exposing 802.11ax (HE) on this
  module; mainline brcmfmac has no HE support at all.
  No AUTOLOAD on purpose: it claims the same PCI ID (14e4:449d) as
  brcmfmac, so enable exactly one of them. STA+AP concurrency supported
  (op_mode=0x5). Requires the AP6275P vendor firmware (bcmdhd-firmware).
endef

define Package/bcmdhd-firmware
  SECTION:=firmware
  CATEGORY:=Firmware
  TITLE:=AP6275P vendor firmware/nvram/clm for bcmdhd
  URL:=https://github.com/rockchip-linux/rkwifibt
endef

define Package/bcmdhd-firmware/description
  Vendor firmware (18.35.387.x), board nvram and CLM blob for the Ampak
  AP6275P, as requested by the bcmdhd driver via request_firmware from
  /lib/firmware. Sourced from Rockchip rkwifibt (AP6275_PCIE).
endef

define Build/Compile
	$(KERNEL_MAKE) M="$(PKG_BUILD_DIR)" modules \
		CONFIG_BCMDHD=m CONFIG_BCMDHD_PCIE=y CONFIG_CFG80211=m CONFIG_BCMDHD_OOB= \
		KBUILD_MODPOST_WARN=1 \
		LINUXINCLUDE="$(BCMDHD_BACKPORTS_INCLUDE) -I$(LINUX_DIR)/arch/$(LINUX_KARCH)/include -I$(LINUX_DIR)/arch/$(LINUX_KARCH)/include/generated -I$(LINUX_DIR)/include -I$(LINUX_DIR)/arch/$(LINUX_KARCH)/include/uapi -I$(LINUX_DIR)/arch/$(LINUX_KARCH)/include/generated/uapi -I$(LINUX_DIR)/include/uapi -I$(LINUX_DIR)/include/generated/uapi -include $(LINUX_DIR)/include/linux/compiler-version.h -include $(LINUX_DIR)/include/linux/kconfig.h"
endef

define Package/bcmdhd-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware
	$(INSTALL_DATA) ./files/fw_bcm43752a2_pcie_ag.bin $(1)/lib/firmware/
	$(INSTALL_DATA) ./files/nvram_ap6275p.txt $(1)/lib/firmware/
	$(INSTALL_DATA) ./files/clm_bcm43752a2_pcie_ag.blob $(1)/lib/firmware/
endef

$(eval $(call KernelPackage,bcmdhd))
$(eval $(call BuildPackage,bcmdhd-firmware))
