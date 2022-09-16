# SPDX-License-Identifier: GPL-2.0-only

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_SOURCE_PROTO:=git
PKG_VERSION:=git
PKG_SOURCE_URL:=https://github.com/edgeforce/u-boot-mt7620.git
PKG_SOURCE_VERSION:=master
PKG_MIRROR_HASH:=skip

PKG_RELEASE:=$(AUTORELEASE)

include $(INCLUDE_DIR)/u-boot.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define U-Boot/Default
  BUILD_TARGET:=ramips
  UBOOT_IMAGE:=u-boot.bin
endef

define U-Boot/mt7620_rfb
  NAME:=MT7620 Reference Board
  UBOOT_CONFIG:=mt7620_rfb
  BUILD_DEVICES:=ralink_mt7620a-evb
  BUILD_TARGET:=ramips
  BUILD_SUBTARGET:=mt7620
  UBOOT_IMAGE:=u-boot-with-spl.bin
endef

define U-Boot/mt7620_mt7530_rfb
  NAME:=MT7620+MT7530 Reference Board
  UBOOT_CONFIG:=mt7620_mt7530_rfb
  BUILD_DEVICES:=ralink_mt7620a-mt7530-evb
  BUILD_TARGET:=ramips
  BUILD_SUBTARGET:=mt7620
  UBOOT_IMAGE:=u-boot-with-spl.bin
endef

define U-Boot/mt7621_rfb
  NAME:=MT7621 Reference Board
  UBOOT_CONFIG:=mt7621_rfb
  BUILD_DEVICES:=mediatek_mt7621-eval-board
  BUILD_TARGET:=ramips
  BUILD_SUBTARGET:=mt7621
  UBOOT_IMAGE:=u-boot-mt7621.bin
endef

define U-Boot/mt7621_nand_rfb
  NAME:=MT7621 Reference Board (NAND)
  UBOOT_CONFIG:=mt7621_nand_rfb
  BUILD_DEVICES:=mediatek_mt7621-eval-board
  BUILD_TARGET:=ramips
  BUILD_SUBTARGET:=mt7621
  UBOOT_IMAGE:=u-boot-mt7621.bin
endef

UBOOT_TARGETS := \
	mt7620_mt7530_rfb \
	mt7620_rfb \
	mt7621_nand_rfb \
	mt7621_rfb

define Build/fip-image
	$(if $(FIP_COMPRESS),\
		xz -f -e -k -9 -C crc32 $(STAGING_DIR_IMAGE)/$(if $(BL2_SOC),$(BL2_SOC),$(BUILD_SUBTARGET))-$(BL2_BOOTDEV)-$(if $(BL2_DDRTYPE),$(BL2_DDRTYPE)-)$(if $(BL2_DDRBLOB),$(BL2_DDRBLOB)ddr-)bl31.bin ;\
		xz -f -e -k -9 -C crc32 $(PKG_BUILD_DIR)/u-boot.bin \
	)
	$(STAGING_DIR_HOST)/bin/fiptool create \
		--soc-fw $(STAGING_DIR_IMAGE)/$(if $(BL2_SOC),$(BL2_SOC),$(BUILD_SUBTARGET))-$(BL2_BOOTDEV)-$(if $(BL2_DDRTYPE),$(BL2_DDRTYPE)-)$(if $(BL2_DDRBLOB),$(BL2_DDRBLOB)ddr-)bl31.bin$(if $(FIP_COMPRESS),.xz) \
		--nt-fw $(PKG_BUILD_DIR)/u-boot.bin$(if $(FIP_COMPRESS),.xz) \
		$(PKG_BUILD_DIR)/u-boot.fip
endef

ifdef CONFIG_TARGET_ramips_mt7621
define Build/Prepare
	$(call Build/Prepare/Default)
ifdef CONFIG_DEBUG
	$(CP) $(DL_DIR)/mt7621_stage_sram.bin $(PKG_BUILD_DIR)/
else
	$(CP) $(DL_DIR)/mt7621_stage_sram_noprint.bin $(PKG_BUILD_DIR)/mt7621_stage_sram.bin
endif
endef
endif

define Build/Configure
	$(call Build/Configure/U-Boot)
	sed -i 's/CONFIG_TOOLS_LIBCRYPTO=y/# CONFIG_TOOLS_LIBCRYPTO is not set/' $(PKG_BUILD_DIR)/.config
endef

define Build/Compile
	$(call Build/Compile/U-Boot)
ifeq ($(UBOOT_IMAGE),u-boot.fip)
	$(call Build/fip-image)
endif
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/$(UBOOT_IMAGE) $(STAGING_DIR_IMAGE)/$(BUILD_VARIANT)-$(UBOOT_IMAGE)
endef

$(eval $(call BuildPackage/U-Boot))