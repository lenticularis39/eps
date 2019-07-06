
NAME = window-maker
VERSION = 0.95.8
SOURCE = https://www.windowmaker.org/pub/source/release

all: proto_install $(NAME).p5m.4.res

clean:
	rm -rf WindowMaker-$(VERSION) WindowMaker-$(VERSION).tar.gz \
		proto_install/ $(NAME).p5m.*

publish: all
	pkgsend publish -s $(REPO) -d proto_install/ window-maker.p5m.4.res

$(NAME).p5m.4.res: $(NAME).p5m.4
	pkgdepend resolve -m $(NAME).p5m.4

$(NAME).p5m.4: $(NAME).p5m.3 $(NAME).mog
	pkgdepend generate -md proto_install/ $(NAME).p5m.3 | pkgfmt > \
		$(NAME).p5m.4

$(NAME).p5m.3: $(NAME).p5m.2 $(NAME).mog
	pkgmogrify window-maker.p5m.2 $(NAME).mog | pkgfmt > \
		$(NAME).p5m.3

$(NAME).p5m.2: $(NAME).p5m.1
	ggrep -Ev 'dir  path=(usr/?\w*/?\w*|usr/share/man|usr/share/man/man1) .*' \
		$(NAME).p5m.1 > $(NAME).p5m.2

$(NAME).p5m.1: proto_install
	pkgsend generate proto_install/ | pkgfmt > window-maker.p5m.1

proto_install: WindowMaker-$(VERSION)
	cd WindowMaker-$(VERSION) && env PATH=/usr/gnu/bin:$(PATH) ./configure \
		--prefix=/usr && gmake && gmake install DESTDIR=`pwd`/../proto_install

WindowMaker-$(VERSION): WindowMaker-$(VERSION).tar.gz
	gzcat WindowMaker-$(VERSION).tar.gz | tar -xvf -

WindowMaker-$(VERSION).tar.gz:
	curl -LO $(SOURCE)/WindowMaker-$(VERSION).tar.gz

