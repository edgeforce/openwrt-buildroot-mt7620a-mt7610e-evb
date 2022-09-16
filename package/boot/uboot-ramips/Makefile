# SPDX-License-Identifier: GPL-2.0-only

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_VERSION:=git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/edgeforce/u-boot-mt7620a-mt7610e-evb.git
PKG_SOURCE_VERSION:=master
PKG_MIRROR_HASH:=skip

#PKG_RELEASE:=$(AUTORELEASE)
#PKG_HASH:=fe732aaf037d9cc3c0909bad8362af366ae964bbdac6913a34081ff4ad565372

include $(INCLUDE_DIR)/u-boot.mk
include $(INCLUDE_DIR)/package.mk

define U-Boot/Default
  BUILD_TARGET:=ramips
  UBOOT_IMAGE:=u-boot.bin
endef

define U-Boot/mt7620a-mt7610e-evb
  NAME:=MT7620a + MT7610e EVB
  UBOOT_CONFIG:=mt7620_rfb
  BUILD_DEVICES:=mt7620a-mt7610e-evb
  BUILD_SUBTARGET:=mt7620
endef

UBOOT_TARGETS := \
	mt7620a-mt7610e-evb

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(UBOOT_IMAGE) $(STAGING_DIR_IMAGE)/$(VARIANT)-$(UBOOT_IMAGE)
endef

$(eval $(call BuildPackage/U-Boot))
