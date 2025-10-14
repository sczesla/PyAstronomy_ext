# meson.build

project(
    'PyAstronomy-ext',
    'c', 'fortran',  # Explicitly enable the Fortran compiler
    version: '2.0.0',
    license: 'MIT',
    # Requires a modern Meson version
    meson_version: '>=0.64.0',
)

# 1. Import the Python module and find the installation used for the build
py_mod = import('python')
py = py_mod.find_installation(pure: false)
py_dep = py.dependency()

# 2. Get the NumPy C-API include directory path
# This is required for compiling modules that interact with NumPy arrays.
incdir_numpy = run_command(py,
    ['-c', 'import numpy; print(numpy.get_include())'],
    check : true
).stdout().strip()

incdir_f2py = run_command(py,
    ['-c', 'import os; os.chdir(".."); import numpy.f2py; print(numpy.f2py.get_include())'],
    check : true
).stdout().strip()

# 3. Create an include directory object for Meson
inc_np = include_directories(incdir_numpy, incdir_f2py)

# 4. Define the 'occultnl' extension (Fortran + .pyf wrapper)
py.extension_module(
    'PyAstronomy_ext.forTrans.occultnl',
    # List all source files
    ['PyAstronomy_ext/forTrans/occultnl.f'],
    # Provide the NumPy includes
    include_directories: inc_np,
    # Link against the Python C-API
    dependencies : py_dep,
    # Specify this is an F2PY module (often implicit, but good practice)
    # Meson/F2PY handle the compilation order (fortran -> C wrapper -> link)
    install : true
)


# 5. Define the 'occultquad' extension
py.extension_module(
    'PyAstronomy_ext.forTrans.occultquad',
    # List all source files
    ['PyAstronomy_ext/forTrans/occultquad.f'],
    include_directories: inc_np,
    dependencies : py_dep,
    install : true
)

# Note: The 'optional=True' logic from setup.py is handled differently in Meson
# and typically managed by the build frontend or by making the extension a subproject.
# For simplicity, this example assumes the necessary Fortran compiler is present.
