ifeq ($(OS),Windows_NT)
    # do 'Doze stuff
else
    UNAME_S := $(shell uname -s)
    ifeq ($(UNAME_S),Linux)
        ifneq ("$(wildcard ${HOME}/.local/share/Steam/steamapps/.)","")
            KSPDIR  := ${HOME}/.local/share/Steam/steamapps/common/Kerbal\ Space\ Program
        else ifneq ("$(wildcard ${HOME}/.local/share/Steam/SteamApps/.)","")
            KSPDIR  := ${HOME}/.local/share/Steam/SteamApps/common/Kerbal\ Space\ Program
        else 
            $(error could not find steam folder)
        endif
        MANAGED := ${KSPDIR}/KSP_Data/Managed/
    endif
    ifeq ($(UNAME_S),Darwin)
        ifneq ("$(wildcard ${HOME}/Library/Application\ Support/Steam/SteamApps/.)", "")
            KSPDIR  := ${HOME}/Library/Application\ Support/Steam/SteamApps/common/Kerbal\ Space\ Program/
        else
            $(error "cannot find steam folder")
        endif
        MANAGED := ${KSPDI6R}/KSP.app/Contents/Data/Managed/
    endif
endif

GAMEDATA		:= ${KSPDIR}/GameData
LOCALGAMEDATA	:= ../GameData/ProceduralParts

TARGETS		:= ProceduralParts.dll

API_FILES := \
    DecouplerTweaker.cs \
    ObjectSerializer.cs \
    ProceduralAbstractShape.cs \
    ProceduralAbstractSoRShape.cs \
    ProceduralHeatshield.cs \
    ProceduralPart.cs \
    ProceduralShapeBezierCone.cs \
    ProceduralShapeCone.cs \
    ProceduralShapeCylinder.cs \
    ProceduralShapePill.cs \
    ProceduralSRB.cs \
    TankContentSwitcher.cs \
    TransformFollower.cs \
    zzVersionChecker.cs \
    ICostModifier.cs \
    VectorUtils.cs \
    KSPUtils.cs \
    UnityUtils.cs \
    EngineWrapper.cs \
    MathUtils.cs \
    $e

RESGEN2	:= resgen2
ifeq ($(OS),Windows_NT)
    GMCS	:= gmcs
else
    GMCS	:= mcs
endif
GIT		:= git
TAR		:= tar
ZIP		:= zip

all: version ${TARGETS}

.PHONY: version
version:
	@../version-gen

info:
	@echo "Modular Fuel Tanks Build Information"
	@echo "    resgen2:  ${RESGEN2}"
	@echo "    gmcs:     ${GMCS}"
	@echo "    git:      ${GIT}"
	@echo "    tar:      ${TAR}"
	@echo "    zip:      ${ZIP}"
	@echo "    KSP Data: ${KSPDIR}"

ProceduralParts.dll: ${API_FILES}
	${GMCS} -unsafe -t:library -lib:${MANAGED} \
		-r:Assembly-CSharp,Assembly-CSharp-firstpass,UnityEngine,UnityEngine.UI \
		-out:$@ $^

clean:
	rm -f ${TARGETS} AssemblyInfo.cs

copy: all
	mkdir -p ${LOCALGAMEDATA}/Plugins
	cp ${TARGETS} ${LOCALGAMEDATA}/Plugins
	cp ../ProceduralParts.version ${LOCALGAMEDATA}

.PHONY: all clean install
