#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#


include $(TOPDIR)/rules.mk

PKG_NAME:=PodFetch
PKG_VERSION:=4.6.0
PKG_RELEASE:=1

PKG_LIBC:=musl
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/SamTV12345/$(PKG_NAME)/archive/refs/tags/v$(PKG_VERSION).tar.gz
PKG_HASH:=5af49c1ef00428fa5fc8e276a2e2203462261ce308e5e648d29b2914ebc837f1
PKG_MAINTAINER := Jennings Liu <jenningsloy318@gmail.com>
PKG_LICENSE:=Apache License 2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)


include $(INCLUDE_DIR)/package.mk
include ../../packages/lang/rust/rust-package.mk

define Package/podfetch
  SECTION:=multimedia
  CATEGORY:=Multimedia
  DEPENDS:=+rust +libpq +libsqlite3
  TITLE:=podfetch
  URL:=https://github.com/SamTV12345/PodFetch/
endef

define Package/podfetch/description
  Podfetch is a self-hosted podcast manager.
  It is a web app that lets you download podcasts and listen to them online. It is written in Rust and uses React for the frontend. 
  It also contains a GPodder integration so you can continue using your current podcast app.
endef
	

define Build/Compile
	cd $(PKG_BUILD_DIR)/ui && npm install && npm run build-github
	cd $(PKG_BUILD_DIR) && \
	PKG_CONFIG_ALLOW_CROSS=1 \
	OPENSSL_DIR=$(STAGING_DIR)/usr/ \
	RUSTFLAGS="-C linker=$(TARGET_CC) -C target-feature=+crt-static" \
	CC=$(TARGET_CC) \
	CFLAGS="$(TARGET_CFLAGS)" \
	TARGET_CFLAGS="$(TARGET_CFLAGS)" \
	CXX=$(TARGET_CXX) \
	CXXFLAGS="$(TARGET_CXXFLAGS)" \
	TARGET_CXXFLAGS="$(TARGET_CXXFLAGS)" \
	cargo build  --release  --target=$(RUSTC_TARGET_ARCH)  --package podfetch --bin podfetch
endef

define Package/podfetch/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/podfetch
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/target/$(RUSTC_TARGET_ARCH)/release/podfetch $(1)/usr/bin/podfetch
	$(CP)  $(PKG_BUILD_DIR)/target/$(RUSTC_TARGET_ARCH)/ui $(1)/etc/podfetch/ui
endef

$(eval $(call BuildPackage,podfetch))
