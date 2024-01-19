#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#


include $(TOPDIR)/rules.mk

PKG_NAME:=podfetch
PKG_VERSION:=4.6.0
PKG_RELEASE:=1
PKG_MAINTAINER := Jennings Liu <jenningsloy318@gmail.com>
PKG_LICENSE:=Apache License 2.0
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/SamTV12345/$(PKG_NAME)/archive/refs/tags/v$(PKG_VERSION).tar.gz
PKG_HASH:=d666f2264afaef79cddc5c6314234d16db22ea0f
PKG_BUILD_DEPENDS = rust postgresql sqlite3

HOST_BUILD_DEPENDS:=rust/host


include $(INCLUDE_DIR)/package.mk
include ../../packages/lang/rust/rust-host-build.mk

define Package/podfetch
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=podfetch
  URL:=https://github.com/SamTV12345/PodFetch/
endef

define Package/podfetch/description
  Podfetch is a self-hosted podcast manager.
  It is a web app that lets you download podcasts and listen to them online. It is written in Rust and uses React for the frontend. 
  It also contains a GPodder integration so you can continue using your current podcast app.
endef
	

define Build/Compile
	cd $(PKG_BUILD_DIR) && cargo rustc --target=$(RUSTC_TARGET_ARCH) --release -- -C linker=$(TARGET_CC_NOCACHE) -C ar=$(TARGET_AR) -C prefer-dynamic -C opt-level=z -C debuginfo=0
	$(TOPDIR)/staging_dir/toolchain-*/bin/aarch64-openwrt-linux-musl-strip -s $(PKG_BUILD_DIR)/target/$(RUSTC_TARGET_ARCH)/release/main
	stat $(PKG_BUILD_DIR)/target/$(RUSTC_TARGET_ARCH)/release/main
endef

define Package/podfetch/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/target/$(RUSTC_TARGET_ARCH)/release/main $(1)/usr/bin/podfetch
endef

$(eval $(call BuildPackage,podfetch))