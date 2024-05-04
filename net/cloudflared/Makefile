include $(TOPDIR)/rules.mk

PKG_NAME:=cloudflared
PKG_VERSION:=2024.4.1
PKG_RELEASE:=1
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_SOURCE_URL:=https://codeload.github.com/cloudflare/cloudflared/tar.gz/${PKG_VERSION}?
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_MD5SUM=62e581b241a8c4769a8a2abedef3ee41

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Growtopia Jaw <growtopiajaw@gmail.com>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/cloudflare/cloudflared
GO_PKG_LDFLAGS:=main.Version=$(PKG_VERSION) -s -w

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/$(PKG_NAME)
  TITLE:=Cloudflare Tunnel client
  URL:=https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/$(PKG_NAME)/description
  Contains the command-line client for Cloudflare Tunnel, a tunneling
  daemon that proxies traffic from the Cloudflare network to your origins.

  This daemon sits between Cloudflare network and your origin (e.g. a
  webserver). Cloudflare attracts client requests and sends them to you
  via this daemon, without requiring you to poke holes on your firewall
  --- your origin can remain as closed as possible.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	$(call GoPackage/Build/Compile)
	# comment this line if upx not present
	$(STAGING_DIR_HOST)/bin/upx --ultra-brute --best $(GO_PKG_BUILD_BIN_DIR)/cloudflared
	chmod +x ./files/etc/init.d/cloudflared
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/cloudflared $(1)/usr/bin/cloudflared
endef
$(eval $(call GoBinPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)))
