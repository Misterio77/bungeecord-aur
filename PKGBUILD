# Maintainer: Johannes Joens <johannes@joens.email>
# Contributer: Misterio <eu@misterio.me>
# Contributer: Gordian Edenhofer <gordian.edenhofer@gmail.com>
# Contributer: Philip Abernethy <chais.z3r0@gmail.com>
# Contributer: sowieso <sowieso@dukun.de>

pkgname=bungeecord
pkgver=1532
pkgrel=1
pkgdesc="BungeeCord is a sophisticated proxy and API designed mainly to teleport players between multiple Minecraft servers."
arch=('any')
url="https://www.spigotmc.org/"
license=('custom')
depends=('java-runtime-headless>=8' 'screen' 'sudo' 'bash' 'awk' 'sed')
optdepends=("tar: needed in order to create world backups"
	"nmap-netcat: required in order to suspend an idle server")
backup=('etc/conf.d/bungeecord')
install="${pkgname}.install"
_name=bungeecord
_subserver=proxy
source=("${_name}.${pkgver}.jar"::"https://ci.md-5.net/job/BungeeCord/${pkgver}/artifact/bootstrap/target/BungeeCord.jar"
	"${_name}-backup@.service"
	"${_name}-backup.timer"
	"${_name}@.service"
	"${_name}.conf"
	"${_name}.sh"
        "${_subserver}.conf"
        "SKIP")
noextract=("${_name}.${pkgver}.jar")
sha512sums=('dda4f0a3caed7e00a2d253fbb0241f9752ad323cd58fbc0efa9b6494d470c99ed47870649f3e85b65cf67fd060f06a00d3be03e4b71a8da6e4a4192a06a2977e'
            '9d64446c1e055f21a2c395313cee7d028d609084044ff1cb0b1872b8182ff213975f1c12f91eb4a35b2e0c621559940703064e392bac828ba1fb0100805272bb'
            '19ee3646bfbace353b65c0373594edb654de11c9671f29cebad3b31109f29f94ade1d529d9f409b0989c376bef9b451585b22a1e0ac4295fcc92d9565f808418'
            'b585e0f0e9a154f23525a2cfc9dbac4fdf07495663cb12ada343d46d5cc3ecf992922ebeab20e529b0f24744d656fa7c97ec7ebaee78f15f0ec8f396f2a74dc4'
            '9b196814e712c5478623686914a5ead497a272a88905159942bd2bff896d46cb8071d89a02abdfe9df8921a681972f880131c50e78ceef31b7048c260fe14568'
            '68b65fff8a5e4c68fba10283e3ede2664cc95e7e6c362511ab67a40c63801ddda72d5554a61e8c53eb36012ea2f46efde102f9109c026890db05ca354214b712'
            '64df0c0da37783cf559b74c5deaaa5b6a3445955a866c90543c3a77084c5d3b9cd66834d950c39e483abf2c571e3702de2d3523b8c7305ac627ebe6872ef9e69'
            'd361e5e8201481c6346ee6a886592c51265112be550d5224f1a7a6e116255c2f1ab8788df579d9b8372ed7bfd19bac4b6e70e00b472642966ab5b319b99a2686')

_server_root="/srv/bungeecord"

package() {
	install -Dm644 ${_name}.conf            "${pkgdir}/etc/conf.d/${_name}"
	install -Dm755 ${_name}.sh              "${pkgdir}/usr/bin/${_name}"
	install -Dm644 ${_name}@.service        "${pkgdir}/usr/lib/systemd/system/${_name}@.service"
	install -Dm644 ${_name}@-backup.service "${pkgdir}/usr/lib/systemd/system/${_name}@-backup.service"
	install -Dm644 ${_name}-backup.timer    "${pkgdir}/usr/lib/systemd/system/${_name}-backup.timer"
        install -Dm644 ${_subserver}.conf       "${pkgdir}${_server_root}/servers/${_subserver}.conf"
	install -Dm644 ${_name}.${pkgver}.jar   "${pkgdir}${_server_root}/servers/${_subserver}/${_name}.${pkgver}.jar"
	ln -s "${_name}.${pkgver}.jar"          "${pkgdir}${_server_root}/servers/${_subserver}/server.jar"

	# Link the log files
	mkdir -p "${pkgdir}/var/log/"
	install -dm2755 "${pkgdir}/${_server_root}/servers/${_subserver}/logs"
	ln -s "${_server_root}/logs" "${pkgdir}/var/log/${_name}"

	# Give the group write permissions and set user or group ID on execution
	chmod g+ws "${pkgdir}${_server_root}"

        install -D ./LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
