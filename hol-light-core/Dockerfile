### ---------------------------------------------------------------------------
### Target: hol-light-core
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-core

COPY --chown=opam:opam --from=holbox-ckpt /home/opam/hol-light/ckpt_core /home/opam/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]
