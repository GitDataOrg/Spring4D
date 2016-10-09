# Build status

As of `20140622`

Summary:

> Delphi XE-XE6 builds are fine, except the iOS ARM builds.  

Note: 
> Need to fix the iOS ARM SDKs on the machine that builds these as these don't seem to be up to date.  
> See <https://bitbucket.org/sglienke/spring4d/issue/31/solve-ios-arm-build-errors>.

	Packages\DelphiXE4\iOSDevice.Debug.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error F2588: Linker error code: 1 ($00000001)
	Packages\DelphiXE4\iOSDevice.Release.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\RAD Studio\11.0\Bin\CodeGear.Delphi.Targets(172,5): error F2588: Linker error code: 1 ($00000001)
	Packages\DelphiXE5\Android.Debug.MSBuildLog.txt
	        Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Packages\DelphiXE5\Android.Release.MSBuildLog.txt
	        Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Packages\DelphiXE5\iOSDevice.Debug.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error F2588: Linker error code: 1 ($00000001)
	Packages\DelphiXE5\iOSDevice.Release.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\RAD Studio\12.0\Bin\CodeGear.Delphi.Targets(187,5): error F2588: Linker error code: 1 ($00000001)
	Packages\DelphiXE6\Android.Debug.MSBuildLog.txt
	        Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Packages\DelphiXE6\Android.Release.MSBuildLog.txt
	        Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Source\Spring.TestRunner.pas(43): error F1026: File not found: 'C:\Users\Developer\Versioned\Spring4D\Tests\Source\dUnit\TextTestRunner.dcu'
	Packages\DelphiXE6\iOSDevice.Debug.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error F2588: Linker error code: 1 ($00000001)
	Packages\DelphiXE6\iOSDevice.Release.MSBuildLog.txt
	        C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	        C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error F2588: Linker error code: 1 ($00000001)
	C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error E2597: ld: file not found: /usr/lib/libiconv.dylib
	C:\Program Files (x86)\Embarcadero\Studio\14.0\Bin\CodeGear.Delphi.Targets(200,5): error F2588: Linker error code: 1 ($00000001)