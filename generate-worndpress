#! /bin/bash

replace_it(){

	echo "Processing $1"

	if [[ $1 != *".jpg"* && $1 != *".gif"* && $1 != *".png"* && $1 != *".svg"* && $1 != *".eot"* && $1 != *".ttf"* && $1 != *".woff"* && $1 != *".swf"* && $1 != *".xap"* && $1 != *".gz"* ]]; then

		# make all the correct changes
		sed -i.bak 's/WordPress/Worndpress/g' "$1"

		# https://github.com/worndpress/Worndpress/commit/a84bd3cdf4820079afcbe8c901d01780444e0773
		sed -i.bak 's/capital_P_dangit/lowercase_p_and_also_an_n_dangit/g' "$1"
		rm "$1".bak
	fi
}

git clone git@github.com:worndpress/Worndpress.git srcgit

wget https://github.com/WordPress/WordPress/archive/master.zip
yes | unzip master
rm master.zip
rm -rf src
mv WordPress-master src

files=$(find src -type f -name '*.*')
for file in $files; do
	replace_it "$file"
done

mv srcgit/.git src/.git

rm -rf srcgit

rm src/wp-content/plugins/hello.php # Re implement https://github.com/worndpress/Worndpress/issues/1

# Re implement https://github.com/worndpress/Worndpress/pull/5
rm src/README.md
wget https://patch-diff.githubusercontent.com/raw/worndpress/Worndpress/pull/5.patch
cd src || exit
patch < ../5.patch
cd ../ || exit
rm 5.patch

# Re implement https://github.com/worndpress/Worndpress/pull/3
wget https://raw.githubusercontent.com/caseypatrickdriscoll/Worndpress/a7b461eb45949661d488c96935a63c67726a1a5f/readme.html
mv readme.html src/readme.html

cd src || exit
git clean -f
git add .

git commit -m "Update code & fix typos."

git push origin master
