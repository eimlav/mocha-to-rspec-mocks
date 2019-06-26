# mocha-to-rspec-mocks

A bunch of regexes for converting mocha tests to rspec-mocks.

These regexes are badly made and cannot be trusted so make sure to keep an eye on your changes at all times. 

If you find a regex doesn't quite meet a condition that it should, make a PR to update it.

A script will be made in due time to run these automatically.

## Regexes

### should

    - provider.should respond_to(:disable)
    + expect(provider).to respond_to(:disable)

**Regex:** `([\w_\-:\.\(\)\[\]?]*)\.should`

**Replace:** `expect($1).to`

### stubs

    - PuppetX::Chocolatey::ChocolateyInstall.stubs(:install_path).returns('c:\dude')
    + allow(PuppetX::Chocolatey::ChocolateyInstall).to receive(:install_path).returns('c:\dude')

**Regex:** `([\w_\-:\.\(\)\[\]?]*)\.stubs`

**Replace:** `allow($1).to receive`

### expects

    - PuppetX::Chocolatey::ChocolateyCommon.expects(:set_env_chocolateyinstall)
    + expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:set_env_chocolateyinstall)

**Regex:** `([\w_\-:\.\(\)\[\]?]*)\.expects`

**Replace:** `expect($1).to receive`

### must

    - source[:name].must eq element_id
    + expect(source[:name]).to eq element_id

**Regex:** `([\w_\-:\.\(\)\[\]?]*)\.must`

**Replace:** `expect($1).to`

### returns

    - expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:choco_config_file).returns(nil)
    + expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:choco_config_file).and_return(nil)

**Regex:** `\.returns`

**Replace:** `.and_return`

### at_most_once

    - expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:set_env_chocolateyinstall).at_most_once
    + expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:set_env_chocolateyinstall).at_most(:once)

**Regex:** `\.at_most_once`

**Replace:** `.at_most(:once)`

### at_least_once
    - expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:set_env_chocolateyinstall).at_least_once
    + expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:set_env_chocolateyinstall).at_least(:once)

**Regex:** `\.at_least_once`

**Replace:** `.at_least(:once)`

### at_least(0)

    - expect(PuppetX::Chocolatey::ChocolateyCommon).to receive(:file_exists?).and_return(true).at_least(0)
    + allow(PuppetX::Chocolatey::ChocolateyCommon).to receive(:file_exists?).and_return(true)

**Regex:** `(?:allow|expect)\((.*)\.at_least\(0\)`

**Replace:** `allow($1`

### raises

    - … '--priority', 0]).raises(Puppet::ExecutionFailure, 'Nooooo')
    + … '--priority', 0]).and_raise(Puppet::ExecutionFailure, 'Nooooo')

**Regex:** `\.raises`

**Replace:** `.and_raise`

### yields

    - expect(provider_class).to receive(:execpipe).yields(StringIO.new(%(package1|1.23\n\package2|2.00\n)))
    + expect(provider_class).to receive(:execpipe).and_yield(StringIO.new(%(package1|1.23\n\package2|2.00\n)))

**Regex:** `\.yields`

**Replace:** `.and_yield`

### to ==

    - expect(provider.send(:latestcmd).drop(1)).to == ['version', 'chocolatey', '| findstr /R "latest" | findstr /V "latestCompare"']
    + expect(provider.send(:latestcmd).drop(1)).to eq ['version', 'chocolatey', '| findstr /R "latest" | findstr /V "latestCompare"']

**Regex:** `to ==`

**Replace:** `to eq`

### before/after :each

    - before :each) do
    + before(:each) do

**Regex:** `\s*(before|after)\s(:each|:all)`

**Replace:** `$1($2)`

