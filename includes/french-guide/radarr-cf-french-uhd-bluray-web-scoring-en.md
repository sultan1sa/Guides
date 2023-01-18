??? abstract "HQ Source Groups - [CLICK TO EXPAND]"
    | Custom Format                                                                                                                 | Score                                                          | Trash ID                                                    |
    | ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------- |
    | [{{ radarr['cf']['french-uhd-bluray-tier-01']['name'] }}](/Radarr/Radarr-collection-of-custom-formats/#fr-uhd-bluray-tier-01) | {{ radarr['cf']['french-uhd-bluray-tier-01']['trash_score'] }} | {{ radarr['cf']['french-uhd-bluray-tier-01']['trash_id'] }} |
    | [{{ radarr['cf']['french-uhd-bluray-tier-02']['name'] }}](/Radarr/Radarr-collection-of-custom-formats/#fr-uhd-bluray-tier-02) | {{ radarr['cf']['french-uhd-bluray-tier-02']['trash_score'] }} | {{ radarr['cf']['french-uhd-bluray-tier-02']['trash_id'] }} |
    | [{{ radarr['cf']['french-web-tier-01']['name'] }}](/Radarr/Radarr-collection-of-custom-formats/#fr-web-tier-01)               | {{ radarr['cf']['french-web-tier-01']['trash_score'] }}        | {{ radarr['cf']['french-web-tier-01']['trash_id'] }}        |
    | [{{ radarr['cf']['french-web-tier-02']['name'] }}](/Radarr/Radarr-collection-of-custom-formats/#fr-web-tier-02)               | {{ radarr['cf']['french-web-tier-02']['trash_score'] }}        | {{ radarr['cf']['french-web-tier-02']['trash_id'] }}        |
    | [{{ radarr['cf']['french-scene']['name'] }}](/Radarr/Radarr-collection-of-custom-formats/#fr-scene-groups)                    | ??????                                                         | {{ radarr['cf']['french-scene']['trash_id'] }}              |

    !!! tip
        French Scene groups are included as a fallback if you really want MULTi release despite a potential loss in quality. Either score the CF to `{{ radarr['cf']['french-scene']['trash_score'] }}` (default) or `0` depending on what you want to achieve.

    ??? info "Original mix (optional)"
        {! include-markdown "../../includes/cf/radarr-cf-uhd-bluray-web-scoring.md" !}