include $(TOPDIR)/rules.mk

PKG_NAME:=pihole
PKG_VERSION:=1.5
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=d86b325dfe4d39b7bc523672bb22f1bdbd12265f
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/pi-hole/pi-hole.git
PKG_SOURCE:=$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=65bd97f3b3cb7aac27454cc70e69c47e74ac175ff527ecf5af5a43a2edea865f


PKG_LICENSE:=EUPL
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/PIHOLE-$(PKG_SOURCE_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/pihole
	SECTION:=net
	CATEGORY:=Network
	TITLE:=A black hole for Internet advertisements
	URL:=https://pi-hole.net/
  	DEPENDS:=pihole-FTL pihole-web +sudo +bash +bind-dig +ca-bundle +coreutils-sort +curl +idn +logrotate +coreutils-install +coreutils-chmod +coreutils-chown +coreutils-mktemp
	PKGARCH:=all
endef

define Package/pihole/description
	The Pi-hole® is a DNS sinkhole that protects your devices from unwanted content, without installing any client-side software.
endef

define Package/pihole/conffiles
	/etc/pihole
endef

define Package/pihole/install
	$(INSTALL_DIR) $(1)/etc/.pihole
	$(CP) -r $(PKG_BUILD_DIR)/files/* $(1)/etc/.pihole

	$(INSTALL_DIR) $(1)/etc/pihole
	$(INSTALL_DATA) ./files/setupVars.conf $(1)/etc/pihole/
	
	echo -n "" > $(1)/etc/pihole/custom.list
	echo -n "openwrt openwrt " > $(1)/etc/pihole/localbranches
	echo -n "v$(PKG_VERSION)-openwrt v$(PKG_VERSION)-openwrt v$(PKG_VERSION)-openwrt" > $(1)/etc/pihole/localversions
	touch $(1)/etc/pihole/GitHubVersions
endef

define Package/pihole/postinst
#!/bin/sh
exec /etc/.pihole/automated\ install/basic-install.sh
endef

define Package/pihole/prerm
#!/bin/sh
exec /etc/.pihole/automated\ install/uninstall.sh
endef

define Build/Prepare
	(echo "Starting prep phase")
	(mkdir -p $(PKG_BUILD_DIR)/files/)
	(cp -R $(PKG_BUILD_DIR)/../pihole-1.0/* $(PKG_BUILD_DIR)/files/)
	(cd $(PKG_BUILD_DIR)/files/ && git apply /root/openwrt-sdk/local/net/pihole/manual_patches/0001-Update-Install-Script.patch)
	$(Build/Prepare/Default)
endef

define Build/Compile
	(echo "Required")
endef

define Build/Install
	(echo "Required")
endef


$(eval $(call BuildPackage,pihole))
