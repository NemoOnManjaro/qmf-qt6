# Maintainer: Chupligin Sergey (NeoChapay) <neochapay@gmail.com>

pkgname=qmf-qt5
pkgver=4.0.4+git135
pkgrel=1
pkgdesc="Qt Messaging Framework (QMF) Qt5"
arch=('x86_64' 'aarch64')
url="https://github.com/sailfishos/messagingframework"
license=('BSD-3-Clause')
depends=('nemo-keepalive'
    'buteo-syncfw'
    'libaccounts-qt'
    'signond')
makedepends=('git')
source=("${url}/archive/refs/tags/$pkgver.tar.gz"
    "0090-fix_missing_librares.patch"
    "0091-use_std_list.patch"
    "0092-fix_build.patch"
    "messageserver5.service"
    "messageserver5-accounts-check.service")

sha256sums=('2765ab5de81544b3b2ad80ebffb0e4bfe0de852b3eb626653fe3073881876dc8'
    'a839b7130aec6f06291aea0ddb2c739f6641a209ab90877be6eb954d49f537e8'
    'becad4f91271b383ad1e687a6aec9410043e0ede58922ab0c158ac9344073977'
    'b2b219fd0a88e26f2f62223cd2142232131924fa3d78a7942677f4598e4e0b5e'
    '052120996fd1b0548b4a40b6a2c36e5d2d94266028f6566b20d9d486d78850c1'
    'd55c0361dd9a17a386157f5de40380b82161d3987c1bd2485ad62883a1296846')

prepare() {
  cd messagingframework-4.0.4-git135
  rm upstream -rf
  git clone https://code.qt.io/qt-labs/messagingframework.git upstream
  cd upstream/
  git checkout ee97df2
  patch -p1 --input="../rpm/0001-Introduce-acceptUntrustedCertificates-configuration.patch"
  patch -p1 --input="../rpm/0002-fix-tests-installation-path.patch"
  patch -p1 --input="../rpm/0003-Accounts-qt-integration.patch"
  patch -p1 --input="../rpm/0004-Start-messageserver-on-system-startup-in-case-there-.patch"
  patch -p1 --input="../rpm/0005-Add-keepalive-timer-to-IMAP-IDLE-service.patch"
  patch -p1 --input="../rpm/0006-Use-Qt5-booster-to-save-memory.patch"
  patch -p1 --input="../rpm/0007-Introduce-QMailAccount-HasPersistentConnection-statu.patch"
  patch -p1 --input="../rpm/0008-Introduce-discovery-of-authentication-from-the-anoun.patch"
  patch -p1 --input="../rpm/0009-Handle-SMTP-response-504.patch"
  patch -p1 --input="../rpm/0010-Recreate-SSO-identity-for-smtp-in-case-of-failure.patch"
  patch -p1 --input="../rpm/0011-Listen-to-sync-schedule-changes-from-buteo-sync-fram.patch"
  patch -p1 --input="../rpm/0012-Do-AUTHENTICATE-PLAIN-in-two-stages.patch"
  patch -p1 --input="../rpm/0013-Prevent-push-enabled-status-to-go-out-of-sync.patch"
  patch -p1 --input="../rpm/0014-Add-signature-settings-in-account.patch"
  patch -p1 --input="../rpm/0015-Use-EightBit-encoding-instead-of-Base64-for-text-typ.patch"
  patch -p1 --input="../rpm/0016-Set-qmf-accountscheck-to-be-run-by-the-booster.patch"
  patch -p1 --input="../rpm/0017-Retrieve-message-lists-based-on-the-folder-sync-poli.patch"
  patch -p1 --input="../rpm/0018-Apply-folder-policy-to-always-on-connection.patch"
  patch -p1 --input="../rpm/0019-Allow-a-service-provided-folder-to-be-set-as-the-sta.patch"
  patch -p1 --input="../rpm/0020-Add-QMailMessage-CalendarCancellation-flag.patch"
  patch -p1 --input="../rpm/0021-Use-a-queued-connection-to-handle-accountsUpdated-si.patch"
  patch -p1 --input="../rpm/0022-Store-socket-in-RuntimeLocation.patch"
  patch -p1 --input="../rpm/0023-Handle-encoded-word-s-containing-partial-characters.patch"
  patch -p1 --input="../rpm/0024-Revert-Fix-bundled-zlib-detection.patch"
  patch -p1 --input="../rpm/0025-Revert-Use-QRandomGenerator-instead-of-qrand.patch"
  patch -p1 --input="../rpm/0026-Revert-Use-range-constructors-for-lists-and-sets.patch"
  patch -p1 --input="../rpm/0027-Revert-Adjust-to-Qt6-QMetaType-API-changes.patch"
  patch -p1 --input="../rpm/0028-Revert-Replace-deprecated-QString-SplitBehavior.patch"
  patch -p1 --input="../rpm/0029-Revert-Fix-disappearance-of-QDateTime-QDate.patch"
  patch -p1 --input="../rpm/0030-Revert-core5compat-addition.patch"
  patch -p1 --input="../rpm/0031-Adjust-qmflist-for-missing-bits-in-5.6.patch"
  patch -p1 --input="../rpm/0032-Revert-loadRelax.patch"
  patch -p1 --input="../rpm/0033-Revert-Set-PLUGIN_CLASS_NAME-in-plugin-.pro-files.patch"
  patch -p1 --input="../rpm/0034-Revert-Bump-version-to-6.0.0-since-we-build-against-.patch"  

  patch -p1 --input="${srcdir}/0090-fix_missing_librares.patch"
  patch -p1 --input="${srcdir}/0091-use_std_list.patch"
  patch -p1 --input="${srcdir}/0092-fix_build.patch"
}

build() {
  cd messagingframework-4.0.4-git135/upstream
  qmake \
    QT_BUILD_PARTS+=tests \
    QMF_INSTALL_ROOT=/usr \
    DEFINES+=QMF_ENABLE_LOGGING \
    DEFINES+=MESSAGESERVER_PLUGINS \
    DEFINES+=QMF_NO_MESSAGE_SERVICE_EDITOR \
    DEFINES+=QMF_NO_WIDGETS \
    DEFINES+=USE_ACCOUNTS_QT \
    DEFINES+=USE_KEEPALIVE \
    DEFINES+=USE_HTML_PARSER \
    CONFIG+=syslog
  make
}

package() {
  cd messagingframework-4.0.4-git135/upstream
  make -j 1 INSTALL_ROOT="${pkgdir}" install
  rm "${pkgdir}/usr/lib/systemd/user/messageserver5.service"
  rm "${pkgdir}/usr/lib/systemd/user/messageserver5-accounts-check.service"

  cp ${srcdir}/messageserver5.service "${pkgdir}/usr/lib/systemd/user/messageserver5.service"
  cp ${srcdir}/messageserver5-accounts-check.service "${pkgdir}/usr/lib/systemd/user/messageserver5-accounts-check.service"

  mkdir -p ${pkgdir}/usr/lib/systemd/user/graphical-session.target.wants/
  ln -s ../messageserver5.service ${pkgdir}/usr/lib/systemd/user/graphical-session.target.wants/
  ln -s ../messageserver5-accounts-check.service ${pkgdir}/usr/lib/systemd/user/graphical-session.target.wants/
