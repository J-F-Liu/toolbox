# rdup - generate a file list suitable for making backups

Home page: https://github.com/miekg/rdup

```
rdup -v /dev/null DIR_TO_BACKUP
```

rdup prints a filelist to standard output. Subsequent programs in a pipe line can be used to actually implement to backup scheme.

The default format rdup uses is: "%p%T %b %u %g %l %s %n\n"
The following escape sequences are understood by rdup:

    'p': '+' if file is new/modified, '-' if removed
    'b': permission bits from lstat(2)
    'm;: the file mode bits, st_mode from lstat(2)
    'u': uid
    'g': gid
    'l': path name length
    's': file size (zero if directory)
    'n': path
    't': time of modification (seconds from epoch)
    'H': the SHA1 hash of the file, all "0" for directories and symlinks
    'T': file type: -, l or d (normail file, symlink or directory)
    'C': the content of the file/link (none for directories)

```
rdup -N timestamp filelist DIR_TO_BACKUP
```

Use the m_time of file `timestamp` as the timestamp to decide what to include in the incremental backup list. If timestamp does not exist, full dump is performed. rdup will create/touch timestamp after it has printed the file list.

The file `filelist` is a internal list rdup writes to, to keep track of which files are in a backup. It is used to calculate the correct incremental dump list, needed for files that are removed, or have a different type. `/dev/null` is used to make a full backup.

rdup uses `/etc/rdup` as directory where the timestamp and filelist files are put, but this is completely overrideable by the user.

```
rdup -N timestamp filelist DIR_TO_BACKUP | rdup-up DIR_TO_DUMP
```

With rdup-up you can update an existing directory structure with the updates as described by rdup.

```
rdup -v /dev/null DIR_OF_BACKUP | rdup-up DIR_TO_RESTORE
```

For **rdup** there is no difference between backups and restores. Making a backup means copying a list of files somewhere else. Restoring files is copying a list of files back to the place they came from. So rdup can be used for both.

# rdedup - a data deduplication engine and a backup software

Home page: https://github.com/dpc/rdedup

```
apt install build-essential gcc-multilib -y
apt install liblzma-dev libsodium-dev
export SODIUM_USE_PKG_CONFIG=1
RUSTFLAGS="-C target-cpu=native" cargo install rdedup
```

```
rdedup --dir /backup init --encryption none
export RDEDUP_DIR=/backup
rdup -x /dev/null DIR_TO_BACKUP | rdedup store BACKUP_NAME
rdedup ls
rdedup rm BACKUP_NAME
rdedup gc
rdedup load BACKUP_NAME | rdup-up DIR_TO_RESTORE
```

rdedup is written in Rust, so is exteremely performant and very reliable.

When storing data through rdedup, a stream of binary, non-compressed, non-encrypted data should be fed to it.
For backup purposes that stream could be output of `tar` or `rdup`.

rdedup will split the stream into smaller chunks using rolling sum, and store each chunk under unique id (sha256 digest) in a special format directory: repo. Then the whole backup will be described as index: a list of digests.

Index will be stored internally just like the data itself. Recursively, this reduces each backup to one unique digest, which is saved under given name.

When restoring data, rdedup will read the index, then restore the data, reading each chunk listed in it.

Thanks to rolling sum chunking scheme, when saving frequently similar data, a lot of common chunks will be reused, saving space.

Every time rdedup saves a new chunk file, its data is encrypted using public key so it can only be decrypted using the corresponding secret key. This way new data can always be added, with full deduplication.

Secure passpharse is required only when restoring data, while adding and deduplicating new data does not.

By using `dev/null` a full backup of the data is created but only store a single reference to duplicate data in the repo.

rdedup will output statistics of the run into the console so you can see the number of new chunks and new bytes that have been written to your repo.

Everything in repo is synchronization friendly. Dropbox, Syncthing and similar should work fine for data synchronization.
