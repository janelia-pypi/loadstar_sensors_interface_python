# This file is generated automatically from metadata.org
# File edits may be overwritten!

.PHONY: upload
upload: metadata package twine add clean

GUIX-SHELL = guix time-machine -C guix/channels.scm -- shell
GUIX-CONTAINER = $(GUIX-SHELL) --container
PORT = /dev/ttyUSB0
GUIX-CONTAINER-PORT = $(GUIX-CONTAINER) --expose=$(PORT)
DEVELOPMENT = -D -f guix/guix.scm
GUIX-CONTAINER-DEVELOPMENT = $(GUIX-CONTAINER) $(DEVELOPMENT)
GUIX-CONTAINER-GUI = $(GUIXCONTAINER-DEVELOPMENT) --preserve='^DISPLAY$$' --preserve='^TERM$$'

.PHONY: shell
shell:
	$(GUIX-SHELL)

.PHONY: container
container:
	$(GUIX-CONTAINER)

.PHONY: exposed-container
exposed-container:
	$(GUIX-CONTAINER-PORT)

.PHONY: dev-container
dev-container:
	$(GUIX-CONTAINER-PORT) $(DEVELOPMENT)

.PHONY: ipython-shell
ipython-shell:
	$(GUIX-CONTAINER-PORT) $(DEVELOPMENT) -- ipython

.PHONY: serial-shell
serial-shell:
	$(GUIX-CONTAINER-PORT) picocom -- picocom -b 9600 -f n -y n -d 8 -p 1 -c $(PORT)

.PHONY: installed-shell
installed-shell:
	$(GUIX-CONTAINER-PORT) -f guix/guix.scm --rebuild-cache

.PHONY: metadata-edits
metadata-edits:
	$(GUIX-CONTAINER-GUI) -- sh -c "emacs -q --no-site-file --no-site-lisp --no-splash -l emacs/init.el --file metadata.org"

.PHONY: metadata
metadata:
	$(GUIX-CONTAINER-DEVELOPMENT) -- sh -c "emacs --batch -Q  -l emacs/init.el --eval '(process-org \"metadata.org\")'"

.PHONY: package
package:
	$(GUIX-CONTAINER-DEVELOPMENT) -- sh -c "python3 setup.py sdist bdist_wheel"

.PHONY: twine
twine:
	$(GUIX-CONTAINER-DEVELOPMENT) --network --expose=$$HOME/.pypirc -- sh -c "twine upload dist/*"

.PHONY: add
add:
	$(GUIX-CONTAINER-DEVELOPMENT) -- sh -c "git add --all"

.PHONY: clean
clean:
	$(GUIX-CONTAINER-DEVELOPMENT) -- sh -c "git clean -xdf"
