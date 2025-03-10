Gamemode: Gunmode
Open(MBaseGameType.h) <br>
Find <br>

    MMATCH_GAMETYPE_DEATHMATCH_SOLO		=0,

Add under <br>

	MMATCH_GAMETYPE_GUNMODE				=18,

Find <br>

    inline bool IsGameRuleDeathMatch(MMATCH_GAMETYPE nGameType)

Add under <br>

    (nGameType == MMATCH_GAMETYPE_GUNMODE) ||

Open(MMatchRuleDeathMatch.cpp) <br>
Find <br>

    class MMatchRuleTeamCTF : public MMatchRule {

Add under <br>

    class MMatchRuleGunMode : public MMatchRule {
    protected:
      bool CheckKillCount(MMatchObject* pOutObject);
      virtual void OnBegin();
      virtual void OnEnd();
      virtual void OnRoundTimeOut();
      virtual bool OnCheckRoundFinish();
      virtual bool RoundCount();
    public:
      MMatchRuleGunMode(MMatchStage* pStage);
      virtual ~MMatchRuleGunMode() { }
      virtual MMATCH_GAMETYPE GetGameType() { return MMATCH_GAMETYPE_GUNMODE; }
    };

Open(MBaseGameType.cpp) <br>
Find <br>

    #define MMATCH_GAMETYPE_DEATHMATCH_SOLO_STR		"Death Match(Solo)"

Add under <br>

    #define MMATCH_GAMETYPE_GUNMODE_STR			    "GunGame"


Find <br>

    _InitGameType(MMATCH_GAMETYPE_DEATHMATCH_SOLO,	MMATCH_GAMETYPE_DEATHMATCH_SOLO,	MMATCH_GAMETYPE_DEATHMATCH_SOLO_STR,  1.0f,			1.0f,					0.0f);

Add under <br>

    _InitGameType(MMATCH_GAMETYPE_GUNMODE, MMATCH_GAMETYPE_GUNMODE, MMATCH_GAMETYPE_DEATHMATCH_SOLO_STR, 1.0f, 1.0f, 0.0f);

Open(MMatchStage.cpp) <br>
Find <br>

	case MMATCH_GAMETYPE_CTF:
		{
			return (new MMatchRuleTeamCTF(this));
		}
		break;

Add under <br>

	case MMATCH_GAMETYPE_GUNMODE:
	{
		return (new MMatchRuleGunMode(this));
	}
	break;

Open(ZCombatInterface.cpp) <br>
Find <br>

    DrawMyWeaponPont(pDC);

Add under <br>

				if (ZGetGame()->GetMatch()->GetMatchType() == MMATCH_GAMETYPE_GUNMODE)
				{
					char buffer[256];
					sprintf(buffer, "[Level %d / %d ]", m_nGunLevel, ZGetGame()->GetMatch()->GetRoundCount());
					TextRelative(pDC, 660.f / 800.f, 480.f / 600.f, buffer);
				}

Open(ZGame.cpp) <br>
Find <br>

		pAttacker->GetStatus().Ref().AddExp(nAttackerExp);
		if (!bSuicide) 
			pAttacker->GetStatus().Ref().AddKills();

Add under <br>

		if (GetMatch()->GetMatchType() == MMATCH_GAMETYPE_GUNMODE && pVictim != m_pMyCharacter)
		{

			ZGetCombatInterface()->GunMode(pAttacker, pAttacker->GetKils());
			ZGetCombatInterface()->AddGunLevel();
		}

Open(ZGameInterface.cpp) <br>
Find <br>

	ZGetGameTypeManager()->SetGameTypeStr( MMATCH_GAMETYPE_DEATHMATCH_SOLO, ZMsg( MSG_MT_DEATHMATCH_SOLO));

Add under <br>

	ZGetGameTypeManager()->SetGameTypeStr( MMATCH_GAMETYPE_GUNMODE, "GunGame");

Open(ZRule.cpp) <br>
Find <br>

	case MMATCH_GAMETYPE_DUEL:
		{
			return (new ZRuleDuel(pMatch));
		}
		break;

Add under <br>

	case MMATCH_GAMETYPE_GUNMODE:
	{
		return (new ZRuleGunMode(pMatch));
	}
	break;

Open(ZStageInterface.cpp) <br>
Find <br>

    case MMATCH_GAMETYPE_DEATHMATCH_SOLO:

Add under <br>

    case MMATCH_GAMETYPE_GUNMODE:


Find <br>

    MAnimation* pAniMapImg = (MAnimation*)pResource->FindWidget( "Stage_MapNameBG");

Add under <br>

    (pSetting->nGameType == MMATCH_GAMETYPE_GUNMODE) ||


Find <br>

    MWidget* pWidget = ZApplication::GetGameInterface()->GetIDLResource()->FindWidget( "StageRoundCountLabel");

Add under <br>

    (pSetting->nGameType == MMATCH_GAMETYPE_GUNMODE) ||

Open(ZCombatInterface.cpp) <br>
Find <br>

	UpdateCombo(pCharacter);

Add under <br>

	m_nGunLevel = AddGunLevel();
	

Find <br>

	m_nBulletSpare = 0;
	m_nBulletCurrMagazine = 0;

Add under <br>

	m_nGunLevel = 1;

Open(ZCombatInterface.h) <br>
Find <br>

	int					m_nMagazine;

Add under <br>

	int                 m_nGunLevel;

Find <br>

	bool IsMenuVisible() { return m_bMenuVisible; }

Add under <br>

	void GunMode(ZCharacter* pCharacter, int GunLevel);
	int AddGunLevel(){ return m_nGunLevel++; };

Open(ZRuleDeathMatch.cpp) <br>
Find <br>

	ZRuleTeamDeathMatch::ZRuleTeamDeathMatch(ZMatch* pMatch) : ZRule(pMatch)
	{

	}

Add under <br>

	ZRuleGunMode::ZRuleGunMode(ZMatch* pMatch) : ZRule(pMatch)
	{

	}

	ZRuleGunMode::~ZRuleGunMode()
	{

	}

Open(ZRuleDeathMatch.cpp) <br>
Find <br>

	class ZRuleSoloDeathMatch : public ZRule

Add under <br>

	class ZRuleGunMode : public ZRule
	{
	public:
		ZRuleGunMode(ZMatch* pMatch);
		virtual ~ZRuleGunMode();
	};

Open(MMatchRuleDeathMatch.cpp) <br>
Find <br>

	MMatchRuleTeamDeath::MMatchRuleTeamDeath(MMatchStage* pStage) : MMatchRule(pStage)
	{
	}

Add under <br>

	// MMatchRuleGunMode ///////////////////////////////////////////////////////////
	MMatchRuleGunMode::MMatchRuleGunMode(MMatchStage* pStage) : MMatchRule(pStage)
	{

	}

	void MMatchRuleGunMode::OnBegin()
	{

	}
	void MMatchRuleGunMode::OnEnd()
	{
	}

	bool MMatchRuleGunMode::RoundCount()
	{
		if (++m_nRoundCount < 1) return true;
		return false;
	}

	bool MMatchRuleGunMode::CheckKillCount(MMatchObject* pOutObject)
	{
		MMatchStage* pStage = GetStage();
		for (MUIDRefCache::iterator i = pStage->GetObjBegin(); i != pStage->GetObjEnd(); i++)
		{
			MMatchObject* pObj = (MMatchObject*)(*i).second;
			if (pObj->GetEnterBattle() == false) continue;

			if (pObj->GetKillCount() >= (unsigned int)pStage->GetStageSetting()->GetRoundMax())
			{
				pOutObject = pObj;
				return true;
			}
		}
		return false;
	}

	bool MMatchRuleGunMode::OnCheckRoundFinish()
	{
		MMatchObject* pObject = NULL;

		if (CheckKillCount(pObject))
		{
			return true;
		}
		return false;
	}

	void MMatchRuleGunMode::OnRoundTimeOut()
	{
		SetRoundArg(MMATCH_ROUNDRESULT_DRAW);
	}


Open(ZCombatInterface.cpp) <br>
Find <br>

	void ZCombatInterface::ShowInfo(bool bVisible)

Add under <br>

	void ZCombatInterface::GunMode(ZCharacter* pCharacter, const int Level)
	{
		//ZGetScreenEffectManager()->AddRoundStart(Level);
		int nItemMelee = 0;
		int nItemPistol = 0;
		int nItemSecun = 0;

		int aleatorio = rand() % 3;
		if (aleatorio == 0)
		{
			nItemMelee = 7;
			nItemPistol = 505005;
			nItemSecun = 5019;
		}
		if (aleatorio == 1)
		{
			nItemMelee = 502013;
			nItemPistol = 504006;
			nItemSecun = 507001;
		}
		if (aleatorio == 2)
		{
			nItemMelee = 18;
			nItemPistol = 506007;
			nItemSecun = 508001; //braker 8
		}

		pCharacter->GetItems()->EquipItem(MMCIP_PRIMARY, nItemPistol);  // Rocket
		pCharacter->ChangeWeapon(MMCIP_PRIMARY);
		pCharacter->ChangeWeapon(MMCIP_PRIMARY);
		pCharacter->GetItems()->EquipItem(MMCIP_SECONDARY, nItemSecun);  // grenade
		pCharacter->ChangeWeapon(MMCIP_SECONDARY);
		pCharacter->GetItems()->EquipItem(MMCIP_MELEE, nItemMelee);  // dagger
		pCharacter->ChangeWeapon(MMCIP_MELEE);
		pCharacter->ChangeWeapon(MMCIP_CUSTOM1);
		pCharacter->ChangeWeapon(MMCIP_CUSTOM2);
		pCharacter->InitItemBullet();

	}


Rebuild Gunz & MatchServer








