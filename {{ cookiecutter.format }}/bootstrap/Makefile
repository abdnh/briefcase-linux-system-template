{%- if cookiecutter.embed_python %}
PBS_PREFIX=../{{ cookiecutter.app_name }}-{{ cookiecutter.version }}/usr/{{ cookiecutter.lib_dir }}/{{ cookiecutter.app_name }}/python
CFLAGS=$(shell $(PBS_PREFIX)/bin/python{{ cookiecutter.python_version|py_tag }}-config --cflags --embed) -DEMBEDDED_PYTHON=1 -DAPP_LIBDIR=\"{{ cookiecutter.lib_dir }}\" -DAPP_NAME=\"{{ cookiecutter.app_name }}\"
LDFLAGS=$(shell $(PBS_PREFIX)/bin/python{{ cookiecutter.python_version|py_tag }}-config --ldflags --embed) -Wl,-rpath,\$$ORIGIN/../{{ cookiecutter.lib_dir }}/{{ cookiecutter.app_name }}/python/lib
{%- else %}
CFLAGS=$(shell /usr/bin/python{{ cookiecutter.python_version|py_tag }}-config --cflags --embed)
LDFLAGS=$(shell /usr/bin/python{{ cookiecutter.python_version|py_tag }}-config --ldflags --embed)
{%- endif %}

all: {{ cookiecutter.app_name }}

pyversion.h:
{%- if cookiecutter.embed_python %}
	$(PBS_PREFIX)/bin/python{{ cookiecutter.python_version|py_tag }} -c "import sys; print(f'#define PY_TAG \"{sys.version_info.major}.{sys.version_info.minor}\"')" > pyversion.h
{%- else %}
	python{{ cookiecutter.python_version|py_tag }} -c "import sys; print(f'#define PY_TAG \"{sys.version_info.major}.{sys.version_info.minor}\"')" > pyversion.h
{%- endif %}

{{ cookiecutter.app_name }}: main.c pyversion.h
	gcc -o {{ cookiecutter.app_name }} $^ $(CFLAGS) $(LDFLAGS) -fPIE

clean:
	rm -rf ../{{ cookiecutter.app_name }}-{{ cookiecutter.version }}/usr/bin
	rm -f {{ cookiecutter.app_name }} pyversion.h

install: all
	mkdir -p ../{{ cookiecutter.app_name }}-{{ cookiecutter.version }}/usr/bin
	cp {{ cookiecutter.app_name }} ../{{ cookiecutter.app_name }}-{{ cookiecutter.version }}/usr/bin

.PHONY: all clean install
