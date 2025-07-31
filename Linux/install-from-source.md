# Installing utilities from Source (If we don't want to use the apt repositories or any utility is not available in the package repository)

## Using `Make` and `Configure` style
```awk
1. Download the package file from the provider website
2. Check Readme.md >> if uses configure then:
3. ./configure && ./ make --prefix=/usr/local #--prefix tells the binary where to look for necassary files
5. sudo make install # install the package into system

# In this process the removal and tracking of package and files is too complex
```

## Building Deb file of the package and install
### 1. `Checkinstall`
```awk
1. ./configure && sudo make --prefix=/usr/local 
2. sudo checkinstall #creates .deb file of the utility
3. sudo dpkg -i package.deb #Install the utility in the system
4. sudo dpkg -r package #uninstall the utility from system
```

### 2. Using `stow` to install utility
```awk
1. ./configure --prefix=/usr/local/stow/htop-3.5.0  && make
2. sudo make install
3. sudo stow /usr/local/stow/htop-3.5.0

# To remove package
1. sudo stow -D htop-3.5.0
2. sudo rm -rf /usr/local/stow/htop-3.5.0
```