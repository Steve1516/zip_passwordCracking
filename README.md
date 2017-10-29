# zip_password_cracking

===========================================

```Python

'''

help('zipfile')

extractall(self, path=None, members=None, pwd=None)
     |      Extract all members from the archive to the current working
     |      directory. `path' specifies a different directory to extract to.
     |      `members' is optional and must be a subset of the list returned
     |      by namelist().


'''

import zipfile

from threading import Thread #多线程测试

import optparse #指定IO

def extractFile(zFile, password):
    try:
        zFile.extractall(pwd=password)
        print("[+] Password = " + password + "\n")
        return password
    except:
        return

def main():
    parser = optparse.OptionParser("usage%prog " + "-f <zipfile> -d <dictionary>")
    parser.add_option("-f", dest="zipname", type="string", help="specify zip file")
    parser.add_option("-d", dest="dirname", type="string", help="specify dictionary file")
    (options, args) = parser.parse_args()
    if (options.zipname == None) | (options.dirname == None):
        print(parser.usage)
        exit(0)
    else:
        zipname = options.zipname
        dirname = options.dirname
    zFile = zipfile.ZipFile(zipname)
    passFile = open(dirname)
    for line in passFile.readlines():
        password = line.strip("\n")
        t = Thread(target=extractFile, args=(zFile,password))
        t.start()

if __name__ == '__main__':
    main()
    

```


`#$ python zip_password_cracking.py -f Zip.zip -d Dir.txt`
